#include "mysql_util.h"

using namespace std;

bool DBRead::connect_to_db(int argc, char *argv[], 
    mysqlpp::Connection& con, const char *kdb) 
{
  if (argc < 1) {
    cerr << "Bad argument count: " << argc << '!' << endl;
    return false;
  }

  if ((argc > 1) && (argv[1][0] == '-')) {
    cout << "usage: " << argv[0] <<
      " [host] [user] [password] [port]" << endl;
    cout << endl << "\tConnects to database ";
    if (kdb) {
      cout << '"' << kdb << '"';
    }
    else {
      cout << "server";
    }
    cout << " on localhost using your user" << endl;
    cout << "\tname and no password by default." << endl << endl;
    return false;
  }

  if (!kdb) {
    kdb = kpcSampleDatabase;
  }

  bool success = false;
  if (argc == 1) {
    success = con.connect(kdb);
  }
  else if (argc == 2) {
    success = con.connect(kdb, argv[1]);
  }
  else if (argc == 3) {
    success = con.connect(kdb, argv[1], argv[2]);
  }
  else if (argc == 4) {
    success = con.connect(kdb, argv[1], argv[2], argv[3]);
  }
  else if (argc >= 5) {
    success = con.connect(kdb, argv[1], argv[2], argv[3], atoi(argv[4]));
  }

  if (!success) 
    cerr << "Database connection failed." << endl << endl;
  else
    cerr << "Connected to Database" << endl;

  return success;
}

bool DBRead::connect(int argc, char *argv[]) 
{   
  // Connect to the sample database.
  // FIXME if we already know the name of the DB then why are we
  // again including it in argc/argv? just simple pass NULL for argv
  // in our case as we dont have any host/username/etc ??
  return (_connected = connect_to_db(argc, argv, _con,  NULL));
}

int DBRead::query (string qry, mysqlpp::StoreQueryResult &res) 
{
  if (!_connected) {
    cerr << "not connected to db!" << endl;
    return 1;
  }

  mysqlpp::Query q = _con.query();
  q << qry;
  if (res = q.store())
    return 0;
  else
    return 1;
}

template <class T>
void DBRead::query_store (string qry, vector<T> &storein) 
{
  if (!_connected) {
    cerr << "not connected to db!" << endl;
    return NULL;
  }

  mysqlpp::Query q = _con.query();
  q << qry;
  q.storein(storein);
}

mysqlpp::Query* DBRead::get_query(string qry) 
{
  if (!_connected) {
    cerr << "not connected to db!" << endl;
    return NULL;
  }

  mysqlpp::Query *q = new mysqlpp::Query(_con.query());
  (*q) << qry;
  return q;
}
