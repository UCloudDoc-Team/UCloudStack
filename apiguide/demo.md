# 15 Python Demo


```python
import hashlib
import urlparse
import urllib
import httplib
 
def _verfy_ac(private_key, params):
    items=params.items()
    items.sort()
 
    params_data = "";
    for key, value in items:
        params_data = params_data + str(key) + str(value)
    params_data = params_data + private_key
 
    sign = hashlib.sha1()
    sign.update(params_data)
    signature = sign.hexdigest()
 
    return signature
	
def test(params):
	httpClient = None
	try:
		params = urllib.urlencode(params)
		headers = {'Content-type': 'application/x-www-form-urlencoded', 
		'Accept': 'text/plain',
		'Host': 'console.pre.ucloudstack.com',
		'Proxy-Connection': 'keep-alive'
		}
		httpClient = httplib.HTTPConnection('console.pre.ucloudstack.com', 80, timeout=10)
		httpClient.request('POST', '/api', params, headers)
		response = httpClient.getresponse()
		print response.status
		print response.reason
		print response.read()
		print response.getheaders()
	except Exception, e:
		print e
	finally:
		if httpClient:
			httpClient.close()

		
if __name__ == '__main__':
	private_key = 'ZwaVOkIzKJaMJMVL5a2_X83R74Hy'
	params = {
	'Action': 'DescribeVMInstance',
	'Offset': 0,
	'Limit': 20,
	'PublicKey': 'STFuzSBjVCtupWReWrRo4DX4pfH'
	}
	signature = _verfy_ac(private_key,params)
	print(signature)
	params['Signature'] = signature
	test(params)
	

```

