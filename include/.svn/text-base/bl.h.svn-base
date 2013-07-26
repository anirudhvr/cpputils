/*
   Part of the SpamTracker codebase
   Copyright (C) 2007  Anirudh Ramachandran <avr@cc.gatech.edu>

   This program is free software; you can redistribute it and/or modify
   it under the terms of the GNU General Public License as published by
   the Free Software Foundation; either version 2 of the License, or
   (at your option) any later version.

   This program is distributed in the hope that it will be useful,
   but WITHOUT ANY WARRANTY; without even the implied warranty of
   MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
   GNU General Public License for more details.

   You should have received a copy of the GNU General Public License along
   with this program; if not, write to the Free Software Foundation, Inc.,
   51 Franklin Street, Fifth Floor, Boston, MA 02110-1301 USA.
*/
#ifndef __BL_H
#define __BL_H

#include <string>
#include <cassert>
#include <sys/socket.h> // for inet_aton/inet_ntoa
#include <netinet/in.h>
#include <arpa/inet.h>
#include <iostream>
#include <sstream>
#include <fstream>

#include "patricia.h"
#include "util.h"

using namespace std;

/*
#define Fill_Prefix(p,f,a,b,mb) \
do { \
if (b <= 0 || b > mb) \
cerr << "invalid key\n"; \
memcpy(&p.add.sin, a, (mb+7)/8); \
p.family = f; \
p.bitlen = b; \
p.ref_count = 0; \
} while (0)
*/


class Patricia {
  private:
    patricia_tree_t *_tree;

    /*
       int _add(int family, char *addr, int bits, void *data) {
       prefix_t prefix;
       patricia_node_t *node;
       Fill_Prefix(prefix, family, addr, bits, _tree->maxbits);
       node = patricia_lookup(_tree, &prefix);
       if ((patricia_node_t *)0 != node) {
       if (node->data) {
       deref_data(node->data);
       }
       node->data = data;
       return 0; // success 
       } else 
       return 1; // failure
       }

       void *_match(int family, char *addr, int bits) {
    //cout << "match called wiht " << addr << " " <<bits << endl;

    prefix_t prefix;
    patricia_node_t *node;
    Fill_Prefix(prefix, family, addr, bits, _tree->maxbits);
    node = patricia_search_best(_tree, &prefix);
    if ((patricia_node_t *)0 != node) {
    return node->data;
    } else {
    return NULL;
    }
    }
    */

  public:
    Patricia();

    ~Patricia();

    int add_string(string &str, void *data) ;

    void *match_string(string &str);
    int remove_string(string &str) ;
};


class Blacklist {
  private:
    Patricia _p;
  public:
    Blacklist() {}

    int add(string &ip_a, unsigned int code) ;
    int add(string &ip_a) ;

    bool lookup(string &ip_a, unsigned int &code) ;

    bool lookup(string &ip_a) ;
    int remove(string &ip_a) ;
};


class Spamhaus : public Blacklist {
  public:
    Spamhaus() {}
    void populate (string file);
};

class ASNLookup : public Blacklist {
    private:
        string _inputfile;
        void* (*_parse)(string line, string &prefix,
                unsigned int &asn);
        void populate();
    public:
        ASNLookup(string inputfile,
            void* (*method)(string ln, string &pref, 
                unsigned int &asn));
        /* if no parse method is supplied, we assume that the
         * format is simply <prefix><space><asn> */
        ASNLookup(string inputfile);
        unsigned int getasn(string ip_a);
};




/*
   int add_string2(string str, void *data) {
   unsigned int mask; 
   string ip;
// find whether this is an IP range or an IP
unsigned int mark;
if ((mark = str.find('/')) < str.length()) {  
stringstream ss;
string mask_s = str.substr(mark+1);
ss << mask_s; ss >> mask;
ip = str.substr(0, mark);
} else {
mask = 32;
ip = str;
}

// dummy in data if no data supplied
if (data == (void*) 0) 
data = (void*) str.c_str();

struct in_addr inaddr;
inet_aton(ip.c_str(), &inaddr);
inaddr.s_addr = ntohl(inaddr.s_addr);
stringstream ss;
string ip_n;
ss << inaddr.s_addr;
ss >> ip_n;

cout << "adding: " << ip.c_str() << "[" << ip_n.c_str() << "] " << mask << ", " << (char*)data << endl;

return _add(AF_INET, (char*)ip_n.c_str(), mask, data);
}

void *match_string2(string str) {
unsigned int mask; 
string ip;
// find whether this is an IP range or an IP
unsigned int mark;
if ((mark = str.find('/')) < str.length()) {  
stringstream ss;
string mask_s = str.substr(mark+1);
ss << mask_s; ss >> mask;
ip = str.substr(0, mark);
} else {
mask = 32;
ip = str;
}

struct in_addr inaddr;
//cout << "[" << ip.c_str() << "]" << endl;
inet_aton(ip.c_str(), &inaddr);

inaddr.s_addr = ntohl(inaddr.s_addr);

stringstream ss;
string ip_n;
//cout << "[" << inaddr.s_addr << "]" << endl;
ss << inaddr.s_addr;
ss >> ip_n;

cout << "searching: " << ip.c_str() << "[" << ip_n.c_str() << "]" << mask << endl;

return _match(AF_INET, (char*)ip_n.c_str(), mask);
}
*/


#endif /* __BL_H */
