Phraser
=======
Simple lexer and parser combinator based on Parsing Expression Grammar


## Example

    require 'phraser'
    include Phraser
    
    Scanner = Phraser::Scanner.new
    
    s = Scanner
    s.token '('
    s.token ')'
    s.token :identifier,  /[a-zA-Z_\.\+\-\']+/
    s.token :whitespace,  /[ \t\r\n]+/
    
    S = rule do
      r = t['('] ^ (S / t[:identifier]).*[:tag] ^ t[')']
      r.action {|e| e[:tag] }
    end
    
    Rule = rule do
      S.+ ^ eof
    end
    
    def self.parse(src)
      lx = Scanner.scan(src).delete_if {|r| r.token == :whitespace }
      Phraser::parse(Rule, lx)
    end
    
    
    require 'pp'
    
    src = DATA.read
    
    doc = parse(src)
    pp doc
    
    
    __END__
    (class MediaContent
      (package serializers.msgpack)
      (field image (array (class Image
          (field uri string)
          (field title string)
          (field width int)
          (field height int)
          (field size int))))
      (field media (class Media
          (field uri string)
          (field title string)
          (field width int)
          (field height int)
          (field format string)
          (field duration long)
          (field size long)
          (field bitrate int)
          (field person (array string))
          (field player int)
          (field copyright string)))
      )

