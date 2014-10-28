PHP fallback function http_build_url()
======================================

For servers without pecl_http package installed.

See http://php.net/manual/en/function.http-build-url.php

Based on the original C code from pecl_http-1.7.6  
http://git.php.net/?p=pecl/http/pecl_http.git;a=blob;f=php_http_url_api.h;h=940db8e61e68e1a544896a691079ad33c5b633ff;hb=b84c1c206944586e218e57a864ab9671194a10cf  
http://git.php.net/?p=pecl/http/pecl_http.git;a=blob;f=http_url_api.c;h=8a70b0f2fb18b3435110b436b7f1f68023cafb15;hb=b84c1c206944586e218e57a864ab9671194a10cf

Some snippets by Sébastien Corne have been used.  
https://github.com/Seebz/Snippets/blob/master/php/http_build_url.php

Version: 1.7.6  
Author: Ivan Tcholakov <ivantcholakov@gmail.com>, 2014  
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

Live test demo: http://iridadesign.com/starter-public-edition-4/www/non-mvc/http_build_url_test.php

**An important note:** Don't use host autodetection (or more generally base url autodetection)
implemented by this function, it relies first on $_SERVER['HTTP_HOST'].  
See http://carlos.bueno.org/2008/06/host-header-injection.html
