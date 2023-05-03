Download Link: https://assignmentchef.com/product/solved-cs205-assignment-5-functions-of-utf8string
<br>
The problem is to write some functions of UTF8string. Since the string is not a normal string, we can use some characters of UTF8 to implement the function. On the other side, we need to override the operator to implement some operators. Such that, we may need to know the knowledge of overrider.




<h2>Part 2-Code</h2>

UTF8string.cpp




<table width="588">

 <tbody>

  <tr>

   <td width="588">UTF8string::~UTF8string(){     str.clear();}string UTF8string::getstring(){     return str;}unsigned long long UTF8string::length()const{     return strlen;}unsigned long long UTF8string::bytes()const{     return str.length();}unsigned long long UTF8string::find(const string substr)const{     unsigned long long count = 0;     unsigned long long index = str.find(substr);     if(index&lt;0) return -1;  //cannot find     unsigned char*p = (unsigned char*)str.c_str();     while(p!=(unsigned char*)&amp;str[index]){         count++;         _utf8_incr(p);}     return count;}void UTF8string::replace(UTF8string to_move,UTF8string to_replace){     unsigned long long start = 0;     unsigned char*p;     unsigned int codepoint;     int bytes_in_char;while(str.find(to_move.getstring(),start)!=string::npos){ //find target in str         bool judge = true;             start = str.find(to_move.getstring(),start);          string prestring = str.substr(0,start);      //prefix string         p = (unsigned char*)prestring.c_str();//judge whether the prefix string is a valid UTF8string         while (*p){codepoint = utf8_to_codepoint(p, &amp;bytes_in_char);if (codepoint) {_utf8_incr(p);             }else{judge = false;break;}}         /*         if prefix string is a valid UTF8string, we replace the string         else we set start+=1, and continue find the next string*/         if(judge){string poststring = str.substr(start+to_move.bytes());             str = prestring + to_replace.getstring() + poststring;             strlen = strlen – to_move.length() + to_replace.length();</td>

  </tr>

 </tbody>

</table>




<table width="588">

 <tbody>

  <tr>

   <td width="588">            start += to_replace.bytes();}else{             start+=1;}} }ostream&amp; operator&lt;&lt;(ostream &amp;out,UTF8string utfstr){     out &lt;&lt; utfstr.getstring();     return out; }UTF8string&amp; operator+(UTF8string str1,const string str2){     UTF8string *utf = new UTF8string(str1.getstring()+str2);     return *utf; }UTF8string&amp; operator+(UTF8string str1,UTF8string str2){UTF8string *utf = new UTF8string(str1.getstring()+str2.getstring());     return *utf; }UTF8string&amp; operator+(const string str1,UTF8string str2){     UTF8string *utf = new UTF8string(str1+str2.getstring());     return *utf;}void UTF8string::operator+=(const string s){     str+=s;     unsigned char*p = (unsigned char *)s.c_str();     while(*p){         strlen++;         _utf8_incr(p);} }void UTF8string::operator+=(UTF8string s){     str+=s.getstring();     strlen+=s.length();}string operator*(UTF8string str1,const unsigned long long n){     string s;for(unsigned long long i=0;i&lt;n;i++){         s+=str1.getstring();}     return s;}string operator*(const unsigned long long n,UTF8string str1){     string s;     for(unsigned long long i=0;i&lt;n;i++){         s+=str1.getstring();}     return s;}string UTF8string::operator!()const{</td>

  </tr>

 </tbody>

</table>

UTF8string.hpp

std::string operator!()const;

};

//overload &lt;&lt;

std::ostream&amp; operator&lt;&lt;(std::ostream&amp; out, const UTF8string str);

//overload


UTF8string&amp; operator+(const UTF8string str1,const UTF8string str2);         UTF8string&amp; operator+(const std::string str1,const UTF8string str2);         UTF8string&amp; operator+(const UTF8string str1,const std::string str2);

//overload *

std::string operator*(const unsigned long long n,const UTF8string str1);         std::string operator*(const UTF8string str1,const unsigned long long n);




#endif

<h2>Part 3-Result &amp; Verification</h2>

<table>

 <tbody>

  <tr>

   <td width="103"></td>

  </tr>

  <tr>

   <td></td>

   <td></td>

  </tr>

 </tbody>

</table>

<strong>Test case #1                                                                                                                                                                              </strong>

<h2>Part 4-Difficulties &amp; Solutions</h2>

1.We set three forms to implement constructors, such that we can support parameter string and pointer to construct.

2.Since we may need to visit private member, we set a function  getstring, and length() and  bytes() to implement it.

3.Since we need to find a string in given UTF8string and give the position, we use find() to find the position in bytes and then use _utf8_incr() to find the position in character.

4.Since we need to implement <sub>replace()</sub>  , first we need to use find() to find the string. Then we need to check if the string is a

valid UTF8string. If the string is a valid UTF8string, then we replace it, else we do nothing.

5.When we need to override operator, we just need to override it in normal way.