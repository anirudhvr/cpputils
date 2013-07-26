#ifndef __UTIL_H
#define __UTIL_H

#include <sys/time.h>
#include <time.h>

#include <cerrno>
#include <string>
#include <iostream>
#include <fstream>
#include <sstream>
#include <stdexcept>
#include <typeinfo>
#include <vector>
#include <iterator>
#include <utility>

#include <unordered_map>

#define MAX(a,b) ((a)>(b) ? (a) : (b))
#define MIN(a,b) ((a)>(b) ? (b) : (a))
#define ERROR(x) \
      error(__FILE__, __LINE__, x)

using namespace std;

/* debugging */
void require (bool condition, string message);
void error (string file, unsigned int line, string message);
void debug1 (FILE *stream, const char *fmt, ...);
void debug2 (FILE *stream, const char *fmt, ...);

/* for a string -> whatever hash map */
/*
namespace __gnu_cxx
{
  template<> struct hash<std::string>
  {
    size_t operator() (const std::string& x) const
    {
      return hash<const char*>() (x.c_str());
    }
  };
}
*/

typedef unordered_map <unsigned int, int> int_int_hash;
typedef unordered_map <std::string, int> str_int_hash;
typedef unordered_map <std::string, float> str_float_hash;
typedef unordered_map <std::string, std::string> str_str_hash;

typedef unordered_map <unsigned int, int>::iterator int_int_hash_iterator;
typedef unordered_map <std::string, int>::iterator str_int_hash_iterator;
typedef unordered_map <std::string, float>::iterator str_float_hash_iterator;
typedef unordered_map <std::string, std::string>::iterator str_str_hash_iterator;

/* utility functions for constructing stream query and result */
//StreamQuery *construct_sql_stream_query (string &sqlquery);

// file operations
bool is_readable (string &filename); 
bool is_readable (char *filename); 
string *get_line (ifstream &in);
bool get_line (ifstream &in, string &s);
string* get_line (FILE *in);
bool get_line (FILE *in, string &s);

// string operations
void chomp (string &s);
int split (string &msg, string &delim, vector<string> &parts);
int split (string &msg, char delim, vector<string> &parts);

/* Timing */
#define MICROSECS 1000000.0
#define MILLISECS 1000.0
#define SECS 1.0
class timer 
{
  private:
    struct timeval begin, end;
    bool time_valid;
    bool begin_called;
    double count;
  public:
    timer() { time_valid = false; begin_called = false; count = 0.0; }
    int start();
    int stop();
    int pause();
    int reset();
    /* return value in milliseconds by default */
    double get_tdiff (double mul_factor = 1000.0, int automatic_reset = 1);
    /* return value in milliseconds by default */
    double get_tdiff_now (double mul_factor = 1000.0);
    /* return value in milliseconds by default */
    double get_tdiff_total (double mul_factor = 1000.0);
};

/* for C-erreors */
extern int errno;

class Exception 
{
  public:
    const char* error;
    Exception(const char* arg) : error(arg) { }
};

class BadConversion : public std::runtime_error {
    public:
        BadConversion(const std::string& s)
            : std::runtime_error("Bad Conversion" + s)
        { }
};

class FatalError : public std::runtime_error {
    public:
        FatalError(const std::string& s) 
            : std::runtime_error("Fatal Error: " + s) {}
};

class ParseError : public std::runtime_error {
    public:
        ParseError(const std::string& s) 
            : std::runtime_error("Parse Error: " + s) {}
};
 
/* convert anything to string */
template<typename T>
inline std::string stringify(T& x)
{
    std::ostringstream o;
    if (!(o << x))
        throw BadConversion(typeid(x).name());
    return o.str();
}

/* convert anything from string */
template<typename T>
inline void convert(const std::string& s, T& x,
        bool failIfLeftoverChars = true)
{
    std::istringstream i(s);
    char c;
    if (!(i >> x) || (failIfLeftoverChars && i.get(c)))
        throw BadConversion(s);
}

/* strips the char c from beginning/end of s */
int strip_char (char c, string &s);
/* 
 * XXX to fix
 
template<class InputIterator>
string join (InputIterator first, InputIterator last, 
        string &joinval);

template<class InputIterator>
string join (InputIterator &first, InputIterator &last, 
        const char *joinval);
        */

/* 
const char* ntoa (unsigned int ip_host_order);
unsigned int ntoa (string ip_ascii);
unsigned int ntoa (const char* ip_ascii);
*/

/*
template <typename KeyType, typename ValueType>
void print_hash (unordered_map<KeyType, ValueType> &hsh, 
    ostream &out)
{
  unordered_map<KeyType, ValueType>::iterator it;
  for (it = hsh.begin(); it != hsh.end(); ++it)
    out << "(" << it->first << ", " << it->second << ") ";
  out << endl;
}
    
*/


#endif /* __UTIL_H */


