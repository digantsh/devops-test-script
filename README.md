# README


## Short Version:

This is how we will test your code:
```bash
$ docker-compose up -d && python /path/to/test.py --domain localhost --port 5000 --cert-path /path/to/localhost.crt
...
All tests pass!
```

## Long Version

### Starting Your Docker Container
This should be all that is needed to start your container:
```bash
$ docker-compose up
```

### Script Requirements

This test script uses [the python requests library](http://docs.python-requests.org/). If you don't already have it, you'll need to install it using [pip](https://pip.pypa.io/en/stable/):
```bash
$ pip install requests
```

### Script Contents

This script simulates API calls to your service like the following:

You can run these SSL enabled curl calls to see it works (in this case we're using port 5000):
```bash
$ curl \
  -X POST \
  -H "Content-Type: application/json" \
  -d '{"message":"foo"}' \
  --cacert localhost.crt \
  https://localhost:5000/messages
{
  "digest": "2c26b46b68ffc68ff99b453c1d30413413422d706483bfa0f98a5e886266e7ae"
}
```

```bash
$ curl \
  --cacert localhost.crt \
  https://localhost:5000/messages/2c26b46b68ffc68ff99b453c1d30413413422d706483bfa0f98a5e886266e7ae
{
  "message": "foo"
}
```

```bash
$ curl \
  --cacert localhost.crt \
  https://localhost:5000/messages/aaaaaaaaaaaaaaaaaaa
{
  "err_msg": "Message not found"
}
```

Note that without the `cacert` flag you will get an `Invalid certificate chain` error.

You can also use `$ wget --ca-certificate=localhost.crt https://localhost:5000/messages/...`.
