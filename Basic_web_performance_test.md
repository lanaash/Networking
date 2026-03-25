Do you know you can use curl for basic web performance testing? 
How much time DNS lookups & redirects, TCP connect, Time to first byte & total time

```
curl -k  -w "@curl-format.txt" -o /dev/null -s  https://YOURWEBSITE 
```

 ```
cat curl-format.txt
time_namelookup: %{time_namelookup}
time_connect: %{time_connect}
time_appconnect: %{time_appconnect}
time_pretransfer: %{time_pretransfer}
time_redirect: %{time_redirect}
time_starttransfer: %{time_starttransfer}
———
time_total: %{time_total}
```
