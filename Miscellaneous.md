1. JSON Parser
```
import java.util.*;

public class JSONParserGeneric {
    private String json;
    private int pos = 0;

    public JSONParserGeneric(String json) {
        this.json = json.trim();
    }

    private char peek() { return pos < json.length() ? json.charAt(pos) : '\0'; }
    private char next() { return pos < json.length() ? json.charAt(pos++) : '\0'; }
    private void skipWhitespace() {
        while (Character.isWhitespace(peek())) pos++;
    }

    public Object parse() throws Exception {
        skipWhitespace();
        char ch = peek();
        if (ch == '{') return parseObject();
        else if (ch == '[') return parseArray();
        else throw new Exception("Invalid JSON");
    }

    private Map<String, Object> parseObject() throws Exception {
        Map<String, Object> obj = new LinkedHashMap<>();
        next(); // skip '{'
        skipWhitespace();
        while (peek() != '}') {
            String key = parseString();
            skipWhitespace();
            if (next() != ':') throw new Exception("Expected ':'");
            skipWhitespace();
            Object value = parseValue();
            obj.put(key, value);
            skipWhitespace();
            if (peek() == ',') next();
            skipWhitespace();
        }
        next(); // skip '}'
        return obj;
    }

    private List<Object> parseArray() throws Exception {
        List<Object> arr = new ArrayList<>();
        next(); // skip '['
        skipWhitespace();
        while (peek() != ']') {
            Object val = parseValue();
            arr.add(val);
            skipWhitespace();
            if (peek() == ',') next();
            skipWhitespace();
        }
        next(); // skip ']'
        return arr;
    }

    private Object parseValue() throws Exception {
        skipWhitespace();
        char ch = peek();
        if (ch == '"') return parseString();
        else if (ch == '{') return parseObject();
        else if (ch == '[') return parseArray();
        else if (Character.isDigit(ch) || ch == '-') return parseNumber();
        else if (json.startsWith("true", pos)) { pos += 4; return true; }
        else if (json.startsWith("false", pos)) { pos += 5; return false; }
        else if (json.startsWith("null", pos)) { pos += 4; return null; }
        else throw new Exception("Invalid value at position " + pos);
    }

    private String parseString() throws Exception {
        if (next() != '"') throw new Exception("Expected '\"'");
        StringBuilder sb = new StringBuilder();
        while (peek() != '"') sb.append(next());
        next(); // skip closing '"'
        return sb.toString();
    }

    private Object parseNumber() {
        int start = pos;
        if (peek() == '-') pos++;
        while (Character.isDigit(peek())) pos++;
        if (peek() == '.') { pos++; while (Character.isDigit(peek())) pos++; }
        String numStr = json.substring(start, pos);
        return numStr.contains(".") ? Double.parseDouble(numStr) : Integer.parseInt(numStr);
    }
}

```
