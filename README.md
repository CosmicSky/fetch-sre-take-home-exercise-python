# Fetch Take Home Exercise

Health Check exercize from Fetch Rewards.

### 1\. Installation and Run Instructions

```python
python -m pip install -r requirements.txt 
```

```python
python main.py <yaml config file>
# python main.py sample.yaml
```

### 2\. Requirements

- Must accept a YAML configuration as command line argument
- YAML format must match that in the sample provided
- Must accurately determine the availability of all endpoints during every check cycle
- Endpoints are only considered available if they meet the following conditions
    - Status code is between 200 and 299
    - Endpoint responds in 500ms or less
- Must return availability by domain
- Must ignore port numbers when determining domain
- Must determine availability cumulatively
- Check cycles must run and log availability results every 15 seconds regardless of the number of endpoints or their response times
- Request default method is GET
- YAML file requires a name and URL - You can assume that the YAML file being used will always be valid

### 3\. Issues and Fixes

1.  Default Request Method Requirement
    line 15 - set default method to GET
    ```
    method = endpoint.get('method', 'GET')
    ```

2.  Timeout Requirement
    line 20 - set request timeout to 500ms
    ```
    response = requests.request(method, url, headers=headers, json=body, timeout=0.5)
    ```

3.  Check Cycle Every 15s
    line 34, 50, 51 - cycle now starts when checking health, not after completing the health checks
    ```
    start_time = time.time()
    sleep_time = max(0, 15 - time.time() + start_time)
    time.sleep(sleep_time)
    ```

4.  Request Name and URL Required
    line 36 - skip request in the YAML if URL or name is not given
    ```
    if endpoint['url'] and endpoint['name']:
    ```

5.  Availability by Domain and Ignore Port
    line 37 - ignore port number and sub-domain to only parse the domain
    ```
    domain = '.'.join(urlparse(endpoint['url']).netloc.split('.')[-2:])
    ```
