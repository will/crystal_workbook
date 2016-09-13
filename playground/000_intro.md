# github.com/will/crystal_workbook

# Will Leinweber

### @leinweber
### bitfission.com
### citusdata.com

## Crystal

### contributor
### github.com/will/crystal-pg

# What is Crystal

* ruby inspired
* compiled with llvm
* self hosted
* static types static dispatch
* still feels rubyish
* very fast

# How fast

### crystal                   ruby
```
# Crystal 0.17.4    |    # ruby 2.3.0p0
# kemal-0.12.0      |    # puma-3.4.0
                    |    # sinatra-1.4.7
                    |
require "kemal"     |    require "sinatra"
                    |
get "/" do          |    get "/" do
  "Hello World!"    |      "Hello World!"
end                 |    end
                    |
Kemal.run           |
```

### wrk -d 1m
```
Crystal                         |  Ruby
                                |
Threed Stats   Avg      Stdev   |  Thread Stats   Avg      Stdev
  Latency   489.98us    0.88us  |    Latency     4.85ms    3.98ms
  Req/Sec    12.14k     2.95k   |    Req/Sec     1.12k   170.08
Requests/sec:  24150.52         |  Requests/sec:   2227.03
Transfer/sec:      2.30MB       |  Transfer/sec:    402.35KB
                                |
RSS 3.0 MB                      |  RSS 164.2 MB
```

## sidekiq
>"I started this project from scratch not knowing the language at all a week ago and had the core job processor running in 3 days."
— http://www.mikeperham.com/2016/05/25/sidekiq-for-crystal/

>"Nearly identical to Sidekiq.rb but look at that execution time: 121µs! In real world code, I’m seeing Crystal execute 5-10x faster than MRI."
— http://www.mikeperham.com/2016/06/14/test-driving-sidekiq-and-crystal/


#### I never wanted to believe my roommate was stealing from his job as a road worker.

```playground
require "base64"
puts Base64.decode_string("QnV0IHdoZW4gSSBnb3QgaG9tZSBhbGwgdGhlIHNpZ25zIHdlcmUgdGhlcmUu\n")
```

# [onwards](010_ruby)
