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

Formal Tests:
-------------

```php
<?php

//------------------------------------------------------------------------------

$code = <<<EOT
// Test 0
echo http_build_url();
// Expected result: The URL of the current page being accessed, without the query string.
EOT;

highlight_string($code);

echo '<code>';
echo 'Result:<br />';
eval($code);
echo '</code>';

//------------------------------------------------------------------------------

$code = <<<EOT
// Test 1
// From http://php.net/manual/en/function.http-build-url.php
echo http_build_url('http://user@www.example.com/pub/index.php?a=b#files',
    array(
        'scheme' => 'ftp',
        'host' => 'ftp.example.com',
        'path' => 'files/current/',
        'query' => 'a=c'
    ),
    HTTP_URL_STRIP_AUTH | HTTP_URL_JOIN_PATH | HTTP_URL_JOIN_QUERY | HTTP_URL_STRIP_FRAGMENT
);
// Expected result:
// ftp://ftp.example.com/pub/files/current/?a=c
EOT;

highlight_string($code);

echo '<code>';
echo 'Result:<br />';
eval($code);
echo '</code>';

//------------------------------------------------------------------------------

$code = <<<EOT
// Test 2
echo http_build_url('http://mike@www.example.com/foo/bar', './baz', HTTP_URL_STRIP_AUTH|HTTP_URL_JOIN_PATH);
// Expected result:
// http://www.example.com/foo/baz
EOT;

highlight_string($code);

echo '<code>';
echo 'Result:<br />';
eval($code);
echo '</code>';

//------------------------------------------------------------------------------

$code = <<<EOT
// Test 3
echo http_build_url('http://mike@www.example.com/foo/bar/', '../baz', HTTP_URL_STRIP_USER|HTTP_URL_JOIN_PATH);
// Expected result:
// http://www.example.com/foo/baz
EOT;

highlight_string($code);

echo '<code>';
echo 'Result:<br />';
eval($code);
echo '</code>';

//------------------------------------------------------------------------------

$code = <<<EOT
// Test 4
echo http_build_url('http://mike:1234@www.example.com/foo/bar/', './../baz', HTTP_URL_STRIP_PASS|HTTP_URL_JOIN_PATH);
// Expected result:
// http://mike@www.example.com/foo/baz
EOT;

highlight_string($code);

echo '<code>';
echo 'Result:<br />';
eval($code);
echo '</code>';

//------------------------------------------------------------------------------

$code = <<<EOT
// Test 5
echo http_build_url('http://www.example.com:8080/foo?a[0]=b#frag', '?a[0]=1&b=c&a[1]=b', HTTP_URL_JOIN_QUERY|HTTP_URL_STRIP_PORT|HTTP_URL_STRIP_FRAGMENT|HTTP_URL_STRIP_PATH);
// Expected result:
// http://www.example.com/?a%5B0%5D=1&a%5B1%5D=b&b=c
EOT;

highlight_string($code);

echo '<code>';
echo 'Result:<br />';
eval($code);
echo '</code>';

//------------------------------------------------------------------------------

$code = <<<EOT
// Test 6
echo http_build_url('/path/?query#anchor');
// Expected similar result:
// http://localhost/path/?query#anchor
EOT;

highlight_string($code);

echo '<code>';
echo 'Result:<br />';
eval($code);
echo '</code>';

//------------------------------------------------------------------------------

$code = <<<EOT
// Test 7
echo http_build_url('/path/?query#anchor', array('scheme' => 'https'));
// Expected similar result:
// https://localhost/path/?query#anchor
EOT;

highlight_string($code);

echo '<code>';
echo 'Result:<br />';
eval($code);
echo '</code>';

//------------------------------------------------------------------------------

$code = <<<EOT
// Test 8
echo http_build_url('/path/?query#anchor', array('scheme' => 'https', 'host' => 'ssl.example.com'));
// Expected result:
// https://ssl.example.com/path/?query#anchor
EOT;

highlight_string($code);

echo '<code>';
echo 'Result:<br />';
eval($code);
echo '</code>';

//------------------------------------------------------------------------------

$code = <<<EOT
// Test 9
echo http_build_url('/path/?query#anchor', array('scheme' => 'ftp', 'host' => 'ftp.example.com', 'port' => 21));
// Expected result:
// ftp://ftp.example.com/path/?query#anchor
EOT;

highlight_string($code);

echo '<code>';
echo 'Result:<br />';
eval($code);
echo '</code>';

//------------------------------------------------------------------------------

$code = <<<EOT
// Test 10
echo http_build_url(parse_url('http://example.org/orig?q=1#f'), parse_url('https://www.example.com:9999/replaced#n'));
// Expected result:
// https://www.example.com:9999/replaced?q=1#n
EOT;

highlight_string($code);

echo '<code>';
echo 'Result:<br />';
eval($code);
echo '</code>';

//------------------------------------------------------------------------------

$code = <<<EOT
// Test 11
echo http_build_url(('http://example.org/orig?q=1#f'), ('https://www.example.com:9999/replaced#n'), 0, \$u);
echo '<br />';
echo nl2br(str_replace(' ', '&nbsp;', print_r(\$u, true)));
// Expected results:
// https://www.example.com:9999/replaced?q=1#n
// Array
// (
//     [scheme] => https
//     [host] => www.example.com
//     [port] => 9999
//     [path] => /replaced
//     [query] => q=1
//     [fragment] => n
// )
EOT;

highlight_string($code);

echo '<code>';
echo 'Results:<br />';
eval($code);
echo '</code>';

//------------------------------------------------------------------------------

$code = <<<EOT
// Test 12
echo http_build_url('page');
// Expected similar result:
// http://localhost/page
// or
// http://localhost/my-path/page
EOT;

highlight_string($code);

echo '<code>';
echo 'Result:<br />';
eval($code);
echo '</code>';

//------------------------------------------------------------------------------

$code = <<<EOT
// Test 13
echo http_build_url('with/some/path/');
// Expected similar result:
// http://localhost/with/some/path/
// or
// http://localhost/my-path/with/some/path/
EOT;

highlight_string($code);

echo '<code>';
echo 'Result:<br />';
eval($code);
echo '</code>';

//------------------------------------------------------------------------------

$code = <<<EOT
// Test 14
echo http_build_url('http://www.example.com/path/to/page/', '../../../another/location', HTTP_URL_JOIN_PATH);
// Expected result:
// http://www.example.com/another/location
EOT;

highlight_string($code);

echo '<code>';
echo 'Result:<br />';
eval($code);
echo '</code>';

//------------------------------------------------------------------------------

$code = <<<EOT
// Test 15
echo http_build_url('http://www.example.com/path/to/page/', '../../../another/location/', HTTP_URL_JOIN_PATH);
// Expected result:
// http://www.example.com/another/location/
EOT;

highlight_string($code);

echo '<code>';
echo 'Result:<br />';
eval($code);
echo '</code>';

//------------------------------------------------------------------------------

$code = <<<EOT
// Test 16
echo http_build_url('http://www.example.com/another/location', '../../path/to/page', HTTP_URL_JOIN_PATH);
// Expected result:
// http://www.example.com/path/to/page
EOT;

highlight_string($code);

echo '<code>';
echo 'Result:<br />';
eval($code);
echo '</code>';

//------------------------------------------------------------------------------

$code = <<<EOT
// Test 17
echo http_build_url('http://www.example.com/path/to/page/', '../another/location/', HTTP_URL_JOIN_PATH);
// Expected result:
http://www.example.com/path/to/another/location/
EOT;

highlight_string($code);

echo '<code>';
echo 'Result:<br />';
eval($code);
echo '</code>';

//------------------------------------------------------------------------------

$code = <<<EOT
// Test 18
echo http_build_url('http://www.example.com/path/to/page/', './another/subpage/', HTTP_URL_JOIN_PATH);
// Expected result:
// http://www.example.com/path/to/page/another/subpage/
EOT;

highlight_string($code);

echo '<code>';
echo 'Result:<br />';
eval($code);
echo '</code>';

//------------------------------------------------------------------------------

$code = <<<EOT
// Test 19
echo http_build_url('http://www.example.com/path/to/page/', '/another/location', HTTP_URL_JOIN_PATH);
// Expected result:
// http://www.example.com/another/location
EOT;

highlight_string($code);

echo '<code>';
echo 'Result:<br />';
eval($code);
echo '</code>';

//------------------------------------------------------------------------------

$code = <<<EOT
// Test 20
echo http_build_url('http://www.example.com/path/to/page/../../../another/location/', null, HTTP_URL_JOIN_PATH);
// Expected result:
// http://www.example.com/another/location/
EOT;

highlight_string($code);

echo '<code>';
echo 'Result:<br />';
eval($code);
echo '</code>';

//------------------------------------------------------------------------------

?>
```
