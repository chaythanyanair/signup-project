#!/usr/bin/env python
#
# Copyright 2007 Google Inc.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#
import webapp2
import re
form="""<head>
<style>
body {background-color:lightgrey}
h1   {color:blue}
p    {color:green}
</style>
</head>

<form method="post" >
    <h1><b>Signup</b></h1>
    <label>Username:<input type="text" Name="username" value="%(uname)s"></label><font style="color: red">%(uerror)s</font><br><br>
    <label>Password:<input type="password" Name="password"></label><font style="color: red">%(perror)s</font><br><br>
    <label>Verify Password:<input type="password" Name="verify"><font style="color: red">%(serror)s</font></label><br><br>
    <label>email(optional):<input type="email" Name="email" value="%(uemail)s"><font style="color: red">%(eerror)s</font></label><br>
    <br>
    <br>
    </div>
    <input type="submit" value="Submit"> 
</form>"""

def verify(name,password,vpassword,email=""):
    USER_RE = re.compile(r"^[a-zA-Z0-9_-]{3,20}$")
    USER_PASS = re.compile(r"^.{3,20}$")
    USER_EMAIL = re.compile(r"^[\S]+@[\S]+\.[\S]+$")
    if not(name):
        return {1:"Username cannot be empty!"}
    if not(password):
        return {2:"Password cannot be empty!"}
    if not(vpassword):
        return {3:"Please confirm your password!"}
    
    if not(USER_RE.match(name)):
        return {1:"Invalid username!"}
    elif not(USER_PASS.match(password) or USER_RE.match(vpassword)):
        return {2:"Invalid password!"}
    elif email:
        if not(USER_EMAIL.match(email)):
            return {4:"Invalid email"} 
    if not(password == vpassword):
        return {3:"Passwords should match!"}
    else:
        return ""

class MainHandler(webapp2.RequestHandler):

    def write_form(self,name="",email="",error={1:"",2:"",3:"",4:""}):
        
        d={"uerror":error.get(1),"perror":error.get(2),"serror":error.get(3),"eerror":error.get(4),"uname":name,"uemail":email}
        for i in d:
            if d[i] == None:
                d[i]=""
        self.response.write(form %d)
        
    def get(self):
        self.write_form()
    def post(self):
        name= self.request.get('username')
        password= self.request.get('password') 
        vpassword= self.request.get('verify')
        email= self.request.get('email') 
        error=verify(name,password,vpassword,email)  
        if error:
            self.write_form(name,email,error)
        else:
            self.redirect("/welcome?name="+name)
            
class WelcomeHandler(webapp2.RequestHandler):
    def get(self):
        name=self.request.get('name')
        self.response.write("Welcome! %s" %name)
    def post(self):
        pass
        #name=self.request.get('name')
        #self.response.write(name)
               
    

app = webapp2.WSGIApplication([
    ('/', MainHandler),
    ('/welcome',WelcomeHandler)
], debug=True)
