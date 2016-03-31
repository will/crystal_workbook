# linking

```playground
@[Link(ldflags: "-lpq -L`pg_config --libdir`")]
lib LibPQ
  fun connect = PQconnectdb(
    conninfo : UInt8*) : Void*
  fun exec = PQexec(
    conn : Void*, query : UInt8*
    ) : Void*
  fun getvalue = PQgetvalue(
    res : Void*, row : Int32,
    column : Int32) : UInt8*
end

conn = LibPQ.connect("postgres:///")
q = "select 'Hello it is ' || now()"
res = LibPQ.exec(conn, q)
p String.new(LibPQ.getvalue(res, 0, 0))
```

```
/tmp ➤ crystal build -s pg.cr
Parse:                             00:00:00.0386820 (  14.78MB)
Semantic (top level):              00:00:00.0169190 (  19.71MB)
Semantic (abstract def check):     00:00:00.0006860 (  19.71MB)
Semantic (type declarations):      00:00:00.0011720 (  19.71MB)
Semantic (initializers):           00:00:00.0005080 (  19.71MB)
Semantic (main):                   00:00:00.0451050 (  26.34MB)
Semantic (cleanup):                00:00:00.0000070 (  26.34MB)
Semantic (recursive struct check): 00:00:00.0002040 (  26.34MB)
Codegen (crystal):                 00:00:00.0547680 (  26.34MB)
Codegen (bc+obj):                  00:00:00.4335010 (  26.34MB)
Codegen (linking):                 00:00:00.0354350 (  26.34MB)
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

# [profiling](060_prof)

