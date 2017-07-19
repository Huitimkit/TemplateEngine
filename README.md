# TemplateEngine
Inspired by [AbsurdJS](http://absurdjs.com/) and in order to see how the template engine works, try to implement a simple template engine myself.

Want to learn more, u can check the original [http://web.jobbole.com/56689/](http://web.jobbole.com/56689/)



### Overview
```
var TemplateEngine = function(html, data) {
    // 捕获所有以<%开头，以%>结尾的片段
    var re     = /<%([^%>]+)?%>/g,
        reExp  = /(^( )?(if|for|else|switch|case|break|{|}))(.*)?/g,
        code   = 'var r = [];\n',
        cursor = 0;

    var add = function(line, js) {
        js? (code += line.match(reExp) ? line + '\n' : 'r.push('+ line +');\n'):
            (code += 'r.push("'+ line.replace(/"/g, '\\"') +'");\n');
    }

    while(match = re.exec(html)) {
        add(html.slice(cursor, match.index))
        add(match[1], true)
        cursor = match.index + match[0].length
    }

    add(html.substr(cursor, html.length - cursor))
    code += 'return r.join("");'

    // 通过字符串构造函数，并传入所需数据，最后返回执行结果
    return new Function(code.replace(/[\r\t\n]/g, '')).apply(data);
}
```