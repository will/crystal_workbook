# linking

```playground
@[Link(ldflags: "-lpq -L`pg_config --libdir`")]
lib LibPQ
  fun connect = PQconnectdb(
      conninfo : UInt8*
  ) : Void*
  fun exec = PQexec(
    conn : Void*, query : UInt8*
  ) : Void*
  fun getvalue = PQgetvalue(
    res : Void*, row : Int32, column : Int32
  ) : UInt8*
end

conn = LibPQ.connect("postgres:///")
q = "select 'Hello it is ' || now()"
res = LibPQ.exec(conn, q)
p String.new(LibPQ.getvalue(res, 0, 0))
```

```
/tmp ➤ crystal build -s pg.cr
Parse:                             00:00:00.0419650 (  11.85MB)
Semantic (top level):              00:00:00.0246300 (  21.07MB)
Semantic (new):                    00:00:00.0005490 (  21.07MB)
Semantic (abstract def check):     00:00:00.0005730 (  21.07MB)
Semantic (type declarations):      00:00:00.0049110 (  21.07MB)
Semantic (cvars initializers):     00:00:00.0007150 (  21.07MB)
Semantic (ivars initializers):     00:00:00.0003280 (  21.07MB)
Semantic (main):                   00:00:00.0475250 (  28.16MB)
Semantic (cleanup):                00:00:00.0000290 (  28.16MB)
Semantic (recursive struct check): 00:00:00.0002890 (  28.16MB)

Codegen (crystal):                 00:00:00.0201080 (  28.16MB)
Codegen (bc+obj):                  00:00:00.1798460 (  28.16MB)
Codegen (linking):                 00:00:00.0406390 (  28.16MB)
/tmp ➤ otool -L pg
pg:
  /usr/local/opt/postgresql/lib/libpq.5.dylib (compatibility version 5.0.0, current version 5.8.0)
  /usr/local/opt/libevent/lib/libevent-2.0.5.dylib (compatibility version 7.0.0, current version 7.9.0)
  /usr/local/opt/pcre/lib/libpcre.1.dylib (compatibility version 4.0.0, current version 4.6.0)
  /usr/lib/libiconv.2.dylib (compatibility version 7.0.0, current version 7.0.0)
  /usr/local/opt/bdw-gc/lib/libgc.1.dylib (compatibility version 2.0.0, current version 2.3.0)
  /usr/lib/libSystem.B.dylib (compatibility version 1.0.0, current version 1226.10.1)
```

### regex
```crystal
class Regex
  def initialize(source, @options = Options::None : Options)
    # PCRE's pattern must have their null characters escaped
    source = source.gsub('\u{0}', "\\0")
    @source = source

    opts = (options | Options::UTF_8 | Options::NO_UTF8_CHECK).value
    @re = LibPCRE.compile(@source, opts, out errptr, out erroffset, nil)
    raise ArgumentError.new("#{String.new(errptr)} at #{erroffset}") if @re.nil?

    @extra = LibPCRE.study(@re, 0, out studyerrptr)
    raise ArgumentError.new("#{String.new(studyerrptr)}") if @extra.nil? && studyerrptr

    LibPCRE.full_info(@re, nil, LibPCRE::INFO_CAPTURECOUNT, out @captures)
  end
end
```

#### I recently had new gutters installed,
```playground
require "base64"
puts Base64.decode_string "dGhlIGNhcnBlbnRlciBzYWlkIGl0IHdhcyBvbiB0aGUgaG91c2U="
```


# [profiling](060_prof)

