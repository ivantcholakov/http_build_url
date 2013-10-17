PHP fallback function http_build_url()
======================================

For servers without pecl_http package installed.

Based on the original C code from pecl_http-1.7.6  
http://svn.php.net/viewvc/pecl/http/tags/RELEASE_1_7_6/php_http_url_api.h?view=markup  
http://svn.php.net/viewvc/pecl/http/tags/RELEASE_1_7_6/http_url_api.c?view=markup

Some snippets by Sébastien Corne have been used.  
https://github.com/Seebz/Snippets/blob/master/php/http_build_url.php

Author: Ivan Tcholakov <ivantcholakov@gmail.com>, 2013  
License: The MIT License, http://opensource.org/licenses/MIT

Usage:  
Place this file on a suitable directory of your PHP system.  
Inside a common bootstrap file within your system insert the following piece of code:  

```php
if (!function_exists('http_build_str') || !function_exists('http_build_url')) {
    require dirname(__FILE__).'/write/your/relative/path/here/http_build_url.php';
}
```

After that, the functions http_build_url() and http_build_str() would be callable.  
A quick test:
```php
echo http_build_url();
```

For testing this fallback implementation, place the file http_build_url_test.php
on the web-server too, and open it with a browser. Don't forget to remove it when
it is no longer needed.