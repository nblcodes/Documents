```java
package com.p6spy.engine.spy.appender;

import com.p6spy.engine.logging.Category;
import org.hibernate.engine.jdbc.internal.FormatStyle;

import java.util.Locale;

public class PrettyLineFormat implements MessageFormattingStrategy {
    @Override
    public String formatMessage(int connectionId, String now, long elapsed, String category, String prepared, String sql, String url) {
        if (sql == null || sql.trim().equals("")) return sql;

        boolean isPretty = prepared.indexOf("\n") >= 0 || prepared.indexOf("\r") >= 0;

        StringBuilder query = new StringBuilder()
                .append("#").append(now).append(" | took ").append(elapsed).append("ms | ").append(category).append(" | connection ").append(connectionId).append(" | url ").append(url);

        FormatStyle formatStyle = FormatStyle.BASIC;
        if (isPretty) {
            formatStyle = FormatStyle.NONE;
        } else if (Category.STATEMENT.getName().equals(category)) {
            String temSql = sql.toLowerCase(Locale.ROOT);
            if (temSql.startsWith("create") || temSql.startsWith("alter") || temSql.startsWith("comment") || temSql.startsWith("drop") || temSql.startsWith("rename") || temSql.startsWith("truncate")) {
                formatStyle = FormatStyle.DDL;
            }
        }

        if (!sql.equals(prepared)) {
            query.append(formatStyle.getFormatter().format(prepared)).append(";\n");
        }

        return query.append(formatStyle.getFormatter().format(sql)).append(";").toString();
//        return now + "|" + elapsed + "|" + category + "|connection " + connectionId + "|url " + url + "|" + P6Util.singleLine(prepared) + "|" + P6Util.singleLine(sql);
//        return "#" + now + " | took " + elapsed + "ms | " + category + " | connection " + connectionId + "| url " + url + "\n" + prepared + "\n" + sql +";";
    }
}
```
