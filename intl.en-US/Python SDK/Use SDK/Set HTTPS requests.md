# Set HTTPS requests {#concept_mws_3dl_zdb .concept}

Alibaba Cloud Python SDK supports using HTTP and HTTPS protocols to send API requests. Most Alibaba Cloud products use the HTTP protocol, whereas Resource Access Manager \(RAM\), Security Token Service \(STS\), and Key Management Service \(KMS\) use the HTTPS protocol.

When using Python SDK, you can specify a protocol \(HTTP or HTTPS\) for a specific request to use, or set a global protocol for all requests to use.

**Note:** The default protocol of a product takes precedence over the global protocol. By default, Resource Access Manager \(RAM\), Security Token Service \(STS\), and Key Management Service \(KMS\) use the HTTPS protocol, and cannot use HTTP protocol.

## Add OpenSSL support {#section_ad3_rkl_zdb .section}

The HTTPS protocol support of Alibaba Cloud Python SDK is based on the OpenSSL support in Python. To use Python SDK to send HTTPS requests, add OpenSSL support for Python.

Run the `python -c "import ssl"` command to make sure the OpenSSL support is added.

The OpenSSL support is added  if no error  is returned.

Run the following command to install OpenSSL if it is not available:

```
pip install pyopenssl
```

## Example: Set HTTP/HTTPS protocol for a request {#section_a3b_dnl_zdb .section}

Refer to the following code example to set HTTPS call for an API:

```
request = CreateInstanceRequest.CreateInstanceRequest()
request.set_protocol_type("https")
# Valid value: "https" or "http"
```

## Example: Set global protocol for all requests {#section_nkx_snl_zdb .section}

Refer to the following code example to set global protocol for all requests:

```
import aliyunsdkcore.request
aliyunsdkcore.request.set_default_protocol_type("https")
# Create the request and call client.do_action_with_exception() to send the request
```

