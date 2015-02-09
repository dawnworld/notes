Test Code
---
## 1. head line
``` cpp
#include <iostream>
#include <string.h>
#include <stdio.h>
#include <signal.h>

#define OTL_ORA11G_R2 // Compile OTL 4.0/OCI11.2
#define OTL_BIGINT long long
const OTL_BIGINT BIGINT_VAL1=12345678901234000LL;

#include "otlv4.h" // include the OTL 4.0 header file
#include "/home/meijx/project/cxx/test/history/pthreadtimeout.h"
#include "/home/meijx/project/cxx/test/history/Printf.h"

otl_connect db; // connect object

using namespace std;

int test_oracle();

void insert() // insert rows into table
{ 
	otl_stream o(100, // buffer size
			"insert into test_tab2 values(:f1<bigint>,:f2<char[31]>)", // SQL statement
			db);
	char tmp[32];

	for(OTL_BIGINT i=BIGINT_VAL1;i<=BIGINT_VAL1+100000;++i){
		int ndx=static_cast<int>(i-BIGINT_VAL1);
		sprintf(tmp,"Name%d",ndx);
		o<<i<<tmp;
	}
}

void select()
{ 
	otl_stream i(50, // buffer size
			"select f1 :#1<bigint>, f2 "
			// the default mapping of f1 needs to be overriden 
			// explicitly when bigint's are used in a combination
			// OTL/OCIx, because the default mapping maps
			// Oracle NUMBERs into double containers, which are not 
			// big enough to hold 64-bit signed integer values.
			"from test_tab2 "
			"where f1>=:f<bigint> "
			"  and f1<=:ff<bigint>",
			// SELECT statement
			db // connect object
			); 
	// create select stream

	OTL_BIGINT f1;
	char f2[31];

	i<<BIGINT_VAL1+3 <<BIGINT_VAL1+9; // assigning :f = 8; :ff = 8
	// SELECT automatically executes when all input variables are
	// assigned. First portion of output rows is fetched to the buffer

	while(!i.eof()){ // while not end-of-data
		i>>f1>>f2;
		cout<<"f1=";
		cout<<f1<<", f2="<<f2<<endl;
	}

}

void getsignal(int sig)
{
	if (sig == SIGABRT)
		Printf("receive SIGABRT[%d]\n", sig);
	if (sig == SIGINT)
	{
		Printf("receive SIGINT[%d]\n", sig);
		exit(-1);
	}

//	db.logoff(); // disconnect from Oracle
//	test_oracle();
}
void dbcancel(void)
{
	db.cancel();
	Printf("cancel ok\n");
}
int test_oracle()
{
	otl_connect::otl_initialize(); // initialize OCI environment
	TIMER timer;
	timer.set(3, dbcancel);
	try
	{

		db.rlogon("ccod/ccod@10.130.41.12:1521/ORCDB"); // connect to Oracle
//		otl_cursor::direct_exec(db, "drop table test_tab2", otl_exception::disabled); // drop table
//		otl_cursor::direct_exec(db, "create table test_tab2(f1 number, f2 varchar2(30))");  // create table
		otl_cursor::direct_exec(db, "delete from test_tab2 where 1=1", otl_exception::disabled); // drop table
		db.commit();

		char cptemp[256];
		for (int i=0; ; i++)
		{
			sprintf(cptemp, "insert into test_tab2 values(%d, 'name%d')", i, i);
			try
			{
				timer.watch_begin();
				otl_cursor::direct_exec(db, cptemp);
				timer.watch_end();
			}
			catch (runtime_error err)
			{
				cout << "ERROR:" << err.what() << endl;
				continue;
			}
			catch (...)
			{
				cout << "other error!" << strerror(errno) << endl;
				continue;
			}
			Printf("%s\n", cptemp);
			db.commit();
			sleep(1);
		}
//		insert(); // insert records into table
//		select(); // select records from table
	}
	catch(otl_exception& p)
	{
		cerr<<p.msg<<endl; // print out error message
		cerr<<p.stm_text<<endl; // print out SQL that caused the error
		cerr<<p.var_info<<endl; // print out the variable that caused the error
	}

	db.logoff(); // disconnect from Oracle
}

int main()
{
	signal(SIGABRT, getsignal);
	signal(SIGINT, getsignal);
	test_oracle();

	return 0;
}
```
## test shell
```bash
ls -al # ls
```