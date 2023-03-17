## Example
```jsonc
{
	"version": 1,
    // Interval of checking and pushing
    // Valid formats: 1m 1h 1d
    // Default: 1m
    // Values less than 1m will be ignored
	"interval": "1m",
    // Check rules
    // Valid types: cpu, mem, net, disk, temp (temperature), swap
    //
    // Threshold
    // format: COMPARE_OPERATOR VALUE UNIT
    // COMPARE_OPERATOR: >, >=, <, <=, =
    // VALUE: int/float: 0.1 1 1.1
    // UNIT: % (percent), m/s (speed), m (size), c (celsius)
    // Speed only valid in per second: b/s k/s, m/s, g/s ...
    // 
    // Matcher:
    // cpu: cpu, cpu0, 1, 2, 3, ...
    // mem: free, used, avail
    // net: eth0, eth1-in, docker-out, ...
    // disk: /, /home, /dev/sda1, ...
    // temp: x86_pkg_temp, x86_core_temp, ...
    // swap: free, used
	"rules": [
		{
			"type": "cpu",
			"threshold": ">=0.1%",
			"matcher": "0"
		},
		{
			"type": "net",
			"threshold": ">=7.7m/s",
			"matcher": "eth0"
		}
	],
    // Push rules
    // type: webhook, ios (more to come)
    // iface: interface for the push type
    // success_body_regex: regex to match the response body
    // success_code: response code to match
	"pushes": [
		{
            // This is a example for QQ Group message
			"type": "webhook",
			"iface": {
				"name": "QQ Group",
                // Webhook url
				"url": "http://localhost:5700",
                // Headers for the request
				"headers": {
					"Authorization": "Bearer YOUR_TOKEN",
					"Content-Type": "application/json"
				},
                // UPPERCASED HTTP method
				"method": "POST",
                // Body for the request
                // {{key}} and {{value}} will be replaced with the key and value of the check result
				"body": {
					"action": "send_group_msg",
					"params": {
						"group_id": 123456789,
						"message": "ServerBox Notification\n{{key}}: {{value}}"
					}
				}
			},
            // Regex to match the response body
            // If the response body matches the regex, the push is considered successful
			"success_body_regex": ".*",
            // If the response code equals, the push is considered successful
			"success_code": 200
		},
        {
            "type": "ios",
            "iface": {
                // You can get it from ServerBox iOS app
                "token": "YOUR_TOKEN",
                // {{key}} and {{value}} will be replaced with the key and value of the check
                "title": "Server Notification",
                // {{key}} and {{value}} will be replaced with the key and value of the check
                "body": "{{key}}: {{value}}"
            }
        }
	]
}
```