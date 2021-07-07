![domainwhois pk-logo](https://user-images.githubusercontent.com/86802146/124763704-b3fb5180-df4d-11eb-9de8-15ed991fbd71.png)



[domainWHOIS.pk](https://domainWHOIS.pk) provides a RESTful [API](https://pknic.domainwhois.pk/api) for WHOIS Lookup of .PK Domain Names which includes information such as Status and Registration Details. It is designed for server-to-server communication between your system and the domainWHOIS.pk network using HTTPs protocol. Query responses are delivered in either JSON, XML or Text format based on your requests which are made via GET/POST methods.
# Making a Request (API Endpoint)
Choose an API endpoint depending on the desired output format from among JSON, XML or Simple Text Output.

**JSON:** <br />`GET | POST   https://pknic.domainwhois.pk/api/json`

**XML:** <br />`GET | POST   https://pknic.domainwhois.pk/api/xml`

### Input parameters
**domain**: The .PK ccTLD Domain name for which the WHOIS data is queried. (Mandatory)
### Sample Query
GET Method with JSON Response:  
`https://pknic.domainwhois.pk/api/json?domain=DOMAIN-NAME-TO-QUERY`

# Getting API Response/Output
Irrespective of the choosen API Output Format, the WHOIS Schema/Response remains the same, as follows:
## Response/Output Parameters
| **Name** | **Details** | **Sample Output** |
| :---         |     :---      |         :---   |
| domain_name   | The queried domain name input string is returned as-is to identify the response.     | google.pk   |
| status     | Domain status indicate Registration staus of the queried domain name.      | Registered, Available, Expired, Reserved, Invalid     |
| registration_date    | **Format**: YYYY-MM-DD <br /> The registration/creation date of the domain. This information is only returned if the domain status is Registered or Expired. | 2000-02-28    |
| expiry_date    | **Format**: YYYY-MM-DD <br /> The date on which the domain has either expired or is set to expire. This information is only returned if the domain status is Registered or Expired.      | 2010-02-28      |
| nameservers    | **Format**: String <br />Array of upto 4 Nameservers set for the queried domain. An empty record is returned if no Nameservers are set.       | ns1.domain.com ns2.domain.com    |
| release_date    | **Format:** YYYY-MM-DD HH:MM:SS  <br />**Timezone:** Pakistan Standard Time (GMT +5)<br /> The date/time on which an Expired domain is available for open registration. This information is only returned if the domain status is 'Expired'.      | 2020-02-28 12:00:00      |
| information     | A brief summary of the returned response containing details about the queried domain name.      | google.pk is Registered.    |

## Sample Response/Output Schema
### json
```
{
	"domain_name": "google.pk",
	"status": "Registered",
	"registration_date": "2005-12-01",
	"expiry_date": "2022-12-01",
	"nameservers": [
		"ns1.google.com",
		"ns2.google.com",
		"ns3.google.com",
		"ns4.google.com"
	],
	"information": "google.pk is Registered"
}
``` 
### xml
``` 
<?xml version="1.0"?>
<whois>
	<domain_name>google.pk</domain_name>
	<status>Registered</status>
	<registration_date>2005-12-01</registration_date>
	<expiry_date>2022-12-01</expiry_date>
	<nameservers>
		<0>ns1.google.com</0>
		<1>ns2.google.com</1>
		<2>ns3.google.com</2>
		<3>ns4.google.com</3>
	</nameservers>
	<information>Domain is already Registered</information>
</whois>
```

## 'Invalid' Domain Status
A Queried domain name which does not comply to the following PKNIC guidelines is returned as Invalid:

* Maximum length of 63 characters (excluding the .pk portion), and the maximum length is 67 characters including .pk suffix.
* Minimum length of 4 characters for second level .PK domains (excluding .pk portion)
* A four character name before .pk must not start or contain any of the PKNIC second level sub-domains (e.g. com, net, org, biz, fam, web, edu, info, gov, gop, gob, gog, gkb, gos gok etc.)
* Domain name can not contain special characters e.g. !@#$%^&*() etc.
* A domain name can not begin with a dash "-", and can not have two consecutive dashes "--"in it.
* A second level domain can not end with one of the PKNIC second level sub-domains (e.g. com, net, edu, org, biz, web, fam, gov, gop, gos, etc.) Exceptions can be made for longer names who's meaning are not easily confused with second level sub-domain space.

# Sample Code
### PHP cURL (GET)
```
$domain_name = "google.pk";
$api_url = "https://pknic.domainwhois.pk/api/json";

$postRequest = array(
	'domain' => $domain_name
);

// create curl resource
$ch = curl_init($api_url);

//Set options for cURL transfer

// The data/domain name to post in a HTTP "POST" operation. 
curl_setopt($ch, CURLOPT_POSTFIELDS, $postRequest);

//return the transfer as a string
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);

// false to stop cURL from verifying the peer's certificate
// curl_setopt($ch, CURLOPT_SSL_VERIFYHOST, false);

// false to disable cURL from verifying
// the common name in the SSL peer's certificate
// curl_setopt($ch, CURLOPT_SSL_VERIFYPEER, false);

// Executing the CURL request and getting the output string
$api_response = curl_exec($ch);

// close curl resource to free up system resources
curl_close($ch);

echo $api_response;	
```
### PHP cURL (POST)
```
$domain_name = "google.pk";
$api_url = "https://pknic.domainwhois.pk/api/json";
    
$postRequest = array(
	'domain' => $domain_name
);

// create curl resource
$ch = curl_init($api_url);

//Set options for cURL transfer

// The data/domain name to post in a HTTP "POST" operation. 
curl_setopt($ch, CURLOPT_POSTFIELDS, $postRequest);

//return the transfer as a string
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);

// false to stop cURL from verifying the peer's certificate
// curl_setopt($ch, CURLOPT_SSL_VERIFYHOST, false);

// false to disable cURL from verifying
// the common name in the SSL peer's certificate
// curl_setopt($ch, CURLOPT_SSL_VERIFYPEER, false);

// Executing the CURL request and getting the output string
$api_response = curl_exec($ch);

// close curl resource to free up system resources
curl_close($ch);

echo $api_response;	
```
### AJAX
```
var domainName = "google.pk";
var apiURL="https://pknic.domainwhois.pk/api/json";

$.ajax({
	type: "GET",
	dataType: 'json',
	crossDomain: true,
	url: apiURL+"?domain="+domainName, 
	success: function(data)
	{
		alert(data);
	},
	error: function(jqXHR, textStatus, errorThrown)
	{
		alert(textStatus);
	}
});
```

# Disclaimer / Terms of Use
[domainWHOIS.pk](https://domainWHOIS.pk)'s [API](https://pknic.domainwhois.pk/api) for .PK Domain Name Lookup is a Third Party [API](https://pknic.domainwhois.pk/api) and it is in no way associated with [PKNIC](https://www.pknic.net.pk/), which is responsible for the administration of the .PK domain name space, including the operation of the DNS for the Root-Servers for .PK domains, and registration and maintenance of all .PK domain names. PKNIC provides domain names in the .PK ccTLD (country code Top Level Domain) namespace.

This [API](https://pknic.domainwhois.pk/api) is provided without any liabiliy or guarantees. [domainWHOIS.pk](https://domainWHOIS.pk) or its team are not liable for any issues arising from the use of this [API](https://pknic.domainwhois.pk/api).
# Support
We are here to listen and assist.
For a quick response, please Contact us at support@domainwhois.pk
