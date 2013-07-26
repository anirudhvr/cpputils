#ifndef __MYSQLUTIL_H
#define __MYSQLUTIL_H

#include <mysql++.h>
#include <iomanip>

class DBRead 
{
  private:
    mysqlpp::Connection _con;
    bool _connected;
    const char* kpcSampleDatabase;
    bool connect_to_db(int argc, char *argv[], mysqlpp::Connection& con,
            const char *kdb);

  public:
    DBRead(std::string db) : _con(false), _connected(false), 
    kpcSampleDatabase((const char *)db.c_str()) 
    {}

    ~DBRead()  {
    // if (_connected) _con.disconnect();
    }

    bool connect(int argc, char *argv[]);

    int query(std::string qry, mysqlpp::StoreQueryResult &res);
    template <typename T> 
      void query_store(std::string qry, std::vector<T> &storein);
    mysqlpp::Query* get_query(std::string qry);
};

#endif
