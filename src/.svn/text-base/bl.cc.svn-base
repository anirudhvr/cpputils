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
#include <cstdlib>
#include <cstdio>
#include "bl.h"

Patricia::Patricia() {
  _tree = New_Patricia(32);
}

Patricia::~Patricia() {
  Destroy_Patricia(_tree, deref_data);
}

int Patricia::add_string(string &str, void *data) {

  // dummy in data if no data supplied
  if (data == (void*) 0) 
    //data = (void*) str.c_str();
    data = (void*) new string(str);

  patricia_node_t *n = make_and_lookup2(_tree, (char*)str.c_str());

  if (n != (patricia_node_t*) 0) {
    free(n->data);
    n->data = data;
    //cout << "data: " << (char*) data << " size: " << sizeof((char*)data) << endl;
  }

  return 0;
  //return _add(AF_INET, (char*)ip_n.c_str(), mask, data);
}

void * Patricia::match_string(string &str) {
  patricia_node_t *found = NULL;
  if ( (found = try_search_best2(_tree, (char*)str.c_str()))  )
    return found->data;
  else
    return NULL;
}

int Patricia::remove_string(string &str) {
  return lookup_then_remove2(_tree, (char*)str.c_str());
}

int Blacklist::add(string &ip_a, unsigned int code) {
  unsigned int *a = (unsigned int*)malloc(sizeof(int));
  *a = code;
  return _p.add_string(ip_a, (void*)a);
}

int Blacklist::add(string &ip_a) {
  return _p.add_string(ip_a, (void*)0);
}

bool Blacklist::lookup(string &ip_a, unsigned int &code) {
  unsigned int *tmp;
  if ( (tmp = (unsigned int*) _p.match_string(ip_a)) != NULL) { 
    code = *tmp;
    return true;
  } else 
    return false;
}

bool Blacklist::lookup(string &ip_a) {
  unsigned int *tmp;
  if ( (tmp = (unsigned int*)_p.match_string(ip_a)) != NULL) 
    return true;
  else 
    return false;
}

int Blacklist::remove(string &ip_a) {
  return _p.remove_string(ip_a);
}

void Spamhaus::populate (string file) {
  ifstream infile;
  infile.open(file.c_str(), ios::in);
  if (infile.is_open()) {
    string line;
    while (infile >> line) {
      add(line);
    }
    infile.close();
  }
}

ASNLookup::ASNLookup(string inputfile) :
    _inputfile(inputfile), _parse(NULL)
{
    populate();
}

ASNLookup::ASNLookup(string inputfile,
        void* (*method)(string ln, string &pref,
            unsigned int &asn)) : 
    _inputfile(inputfile), _parse(method)
{
    populate();
}

void ASNLookup::populate()
{
    ifstream fh;
    char ln[512];
    string pfx;
    vector<string> pieces;
    unsigned int linecount = 0;

    unsigned int code;
    string line;
    if (!is_readable(_inputfile))
        throw new FatalError ("Cant open prefix:asn file");

    fh.open(_inputfile.c_str(), ios::in);
    while (fh.good()) {
        fh.getline(ln, 512);
        if (fh.eof()) break;
        line = ln; /* not sure how bad this is */
        chomp(line);
        if (_parse == NULL) { /* default parse method */
            if (split(line, ' ', pieces) != 2) {
                std::cerr << "Error parsing ASN file: "
                    "found weird line: " << line << endl;
                continue;
            }
            convert (pieces[1], code, false); /* convert asn to intger */
            add(pieces[0] /* prefix */, code); /* base calss method */
        } else { /* they've provided a parse routine */
            if (!_parse(line, pfx, code))
                throw new ParseError ("Error parsing ASN file "
                        "with user-provided _parse");
            add(pfx, code); /* base class method */
        }
    }

    fh.close();
}

unsigned int ASNLookup::getasn(string ip_a)
{
    unsigned int code;
    if (lookup(ip_a, code))
        return code;
    else
        return 0;
}


#if 0

#include <iostream>
#include "bl.h"

int main()
{
    ASNLookup a("/export/home/avr/origin_as_mapping.txt");
    std::cout << a.getasn("143.215.129.45") << " " << a.getasn("130.207.1.1") 
        << std::endl;

    return 0;
}

#endif

