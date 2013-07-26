#include "util.h"
#include <cstdio>
#include <cstdarg>
#include <cstdlib>
#include <iterator>

/* utility functions for constructing stream query and result */
/* XXX: does nothing at the moment, but the intent is to have
 * this function automatically parse an sql query string and construct
 * the StreamQuery object 
 * XXX: move elsewhere */
 /*
StreamQuery *
construct_sql_stream_query (string &sqlquery)
{
    return NULL;
}
*/

/*
 * Debugging prototypes
 */
void error (string file, unsigned int line, string message)
{
    std::cerr << "Error: " << file << ":"
        << line << " - " << message << std::endl;
}


void require (bool condition, string message)
{
#ifndef NDEBUG
    if (!condition) {
        std::cerr << message << endl;
        abort();
    }
#endif
}
                  

void
debug1 (FILE *stream, const char *fmt, ...) {
#ifdef DEBUG1
    va_list ap;
    va_start (ap, fmt);
    vfprintf (stream, fmt, ap);
    va_end (ap);
#endif
}

void
debug2 (FILE *stream, const char *fmt, ...) {
#ifdef DEBUG2
    fprintf (stderr, "coming here\n");
    va_list ap;
    va_start (ap, fmt);
    vfprintf (stream, fmt, ap);
    va_end (ap);
#endif
}


bool 
is_readable (string &input_file)
{
  return (access(input_file.c_str(), R_OK) < 0) ? false : true;
}

bool 
is_readable (char * file) 
{
  return ( (access (file, R_OK) < 0) ? false : true );
}

string* 
get_line (ifstream &in) {
    string *s = new string("");
    std::getline (in, *s);
    return s;
}

bool get_line(ifstream &in, string &s) {
    if (std::getline (in, s) < 0) 
        return false;
    else 
        return true;
}

string* 
get_line(FILE *in) {
  char *lineptr = NULL;
  size_t size;
  if (getline (&lineptr, &size, in) < 0)
    return NULL;
  else {
    require ( (lineptr != NULL), "Null line");
    string *toreturn = new string(lineptr);
    if (lineptr) free (lineptr);
    return toreturn;
  }
}

bool 
get_line(FILE *in, string &s) {
  char *lineptr = NULL;
  size_t size;
  s.erase();
  if (getline (&lineptr, &size, in) > 0) {
    require ( (lineptr != NULL), "Null line");
    s = lineptr;
    if (lineptr) free (lineptr);
    return true;
  } else
    return false;
}

void chomp (string &s) {
  unsigned int t;
  if ( (t = s.find_first_of('\n')) < s.length()) { // found newline
    s.erase(t, s.length() - 1);
  }
}

int 
split (string &msg, string &delim, vector<string> &parts) {
  unsigned int msg_length = msg.length();
  unsigned int delim_length = delim.length();
  unsigned int offset = 0;
  unsigned int rval = 0;
  unsigned int off_curr;

  if (! (msg_length && delim_length && msg_length >= delim_length) ) {
    std::cout << "msg: " << msg << "; msg len: " << msg_length 
      << "; delim len: " << delim_length << endl;
    abort();
  }
  parts.clear();
  while ( ((off_curr = msg.find (delim, offset)) 
        < msg_length) ) {
    if (off_curr > offset) {
      parts.push_back(msg.substr(offset, off_curr - offset));
      ++rval;
    }
    offset = off_curr + delim_length;
  }

  if (offset < msg_length && ++rval)
      parts.push_back(msg.substr(offset, msg_length - offset));

  return rval;
}

int 
split (string &msg, char delim, vector<string> &parts) {
  char tmp[] = {delim, '\0'};
  string delim_str(tmp);
  return split (msg, delim_str, parts);
}

/* 
 * Timing related functions
 */

int timer::start () {
    if (time_valid) time_valid = false;
    if (!gettimeofday (&begin, NULL)) {
        begin_called = true;
        return 1;
    }
    return 0;
}

int timer::stop () {
    if (time_valid) time_valid = false;
    if (!begin_called) {
        ERROR ("Timer not started yet");
        return 0;
    }

    if (!gettimeofday (&end, NULL)) {
      time_valid = true; /* set */
      begin_called = false; /* reset */
      return 1;
    }
    return 0;
}

int timer::pause() {
    if (!begin_called) {
        ERROR ("Timer not started yet");
        return 0;
    }

    if (!gettimeofday (&end, NULL)) {
      time_valid = true; /* set */
      begin_called = false; /* reset */
      count += get_tdiff();
      return 1;
    }
    return 0;
}

int timer::reset () {
    time_valid = false;
    begin_called = false;
    return 1;
}

/* return value in milliseconds by default */
double timer::get_tdiff (double mul_factor, int automatic_reset) {
    if (!time_valid)  {
        ERROR ("Timer not started/finished counting");
        return 0.0;
    }
    time_t sec_diff = end.tv_sec - begin.tv_sec;
    suseconds_t usec_diff = end.tv_usec - begin.tv_usec;

    if (automatic_reset) reset();

    return ( (double)sec_diff * mul_factor + 
            (double)usec_diff / (1000000.0/mul_factor) );
}

/* return value in milliseconds by default */
double timer::get_tdiff_total (double mul_factor) {
    if (!time_valid)  {
        ERROR ("Timer not started/finished counting");
        return 0.0;
    }
    return count;
}




/* return value in milliseconds by default */
double timer::get_tdiff_now (double mul_factor) {
    struct timeval now;

    if (!begin_called)  {
        ERROR ("Timer not started counting");
        return 0.0;
    }
    if (!gettimeofday (&now, NULL)) {
        time_t sec_diff = now.tv_sec - begin.tv_sec;
        suseconds_t usec_diff = now.tv_usec - begin.tv_usec;
        return ( (double)sec_diff * mul_factor 
        + (double)usec_diff / (1000000.0/mul_factor) );
    }

    return 0.0;
}


/*
 * String functions 
 */

/* 0 = did not strip */
int strip_char (char c, string &s)
{
    int rcount = 0;
    if (s.find_first_of(c) == 0 && ++rcount)
        s.erase(s.begin());
    if (s.find_last_of(c) == s.size() - 1 && ++rcount)
        s.erase(s.end()-1);

    return rcount;
}

void print_hash (str_str_hash &hsh)
{
    str_str_hash::iterator it;
    for (it = hsh.begin(); it != hsh.end(); ++it)
        cout << "(" << it->first  << ":" << it->second << ") ";
    cout << endl;
}


/*
template<class InputIterator>
string join (InputIterator first, InputIterator last, 
        string &joinval)
{
    stringstream ss;
    InputIterator tmp = first;
    while (tmp++ != last)
        ss << *tmp << joinval;

    return ss.str();
}

template<class InputIterator>
string join (InputIterator &first, InputIterator &last, 
        const char *joinval)
{
    stringstream ss;
    InputIterator tmp = first;
    while (tmp++ != last)
        ss << *tmp << joinval;

    return ss.str();
}

*/


/* some other utility functions */


