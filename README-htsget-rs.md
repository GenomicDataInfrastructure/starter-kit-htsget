# HTSGET

To run the htsget-rs server first start the docker-compose-demo.yml version of
the [starter kit Storage &
Interfaces](https://github.com/GenomicDataInfrastructure/starter-kit-storage-and-interfaces)
product.

Then you can start the htsget server by running

```bash
docker compose -f docker-compose.yml up -d
```

## Simple test

This test firsts gets a valid jwt token so we're allowed to access a file, the
second command gets the reads from the htsget endpoint. htsget then uses the
download service in the storage-and-interfaces deployment to get to the files.

```bash
token=$(curl -s -k https://localhost:8080/tokens | jq -r '.[0]')
curl -v -H "Authorization: Bearer $token" -k http://localhost:8088/reads/EGAD74900000101/NA12878_20k_b37
```
