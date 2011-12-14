Below is an example of configuring Gente to be served by Apache as a CGI script at  _https://example.com/gente/_. It requires your users to authenticate to Apache using their current LDAP password before they are allowed to access Gente to change that password.

    # Rewrite all requests for paths below gente/ back to gente/
    # Combined with the DirectoryIndex below this gives Gente a pretty URL
    # and prevents access to anything besides the Gente application
    ReWriteRule ^/gente/.+ /gente/ [R,L]
    
    <Directory /var/www/example.com/html/gente>
      Options +ExecCGI
      AddHandler cgi-script .pl
      DirectoryIndex gente.pl
    </Directory>

    <Location /gente>
      SSLRequireSSL
      AuthzLDAPAuthoritative On
      AuthType Basic
      AuthBasicProvider ldap
      AuthName "Example Login"
      AuthLDAPBindDN "uid=exampleuser,ou=People,dc=example,dc=com"
      AuthLDAPBindPassword examplepassword
      AuthLDAPURL "ldap://ldap.example.com/ou=People,dc=example,dc=com?uid?sub"
      Require valid-user
    </Location>
