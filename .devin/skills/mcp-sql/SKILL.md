---
name: mcp-sql
description: Work with SQL databases using MCP (Model Context Protocol) servers
argument-hint: "[query|operation]"
allowed-tools:
  - read
  - grep
  - glob
  - mcp_list_servers
  - mcp_list_tools
  - mcp_call_tool
  - mcp_read_resource
triggers:
  - user
  - model
---

Help work with SQL databases using MCP (Model Context Protocol) servers. Follow this systematic approach:

## 1. Check Available MCP Servers

First, list available MCP servers to see what SQL/database servers are configured:
- Use `mcp_list_servers` to discover available servers
- Look for database-related servers (PostgreSQL, MySQL, SQL Server, SQLite, etc.)
- Check if a generic SQL MCP server is available

## 2. List Available Tools for SQL Server

For each relevant SQL MCP server:
- Use `mcp_list_tools` with the server name to see available tools
- Identify tools for:
  - Query execution
  - Database schema inspection
  - Transaction management
  - Connection management
  - Data manipulation (INSERT, UPDATE, DELETE)
  - Metadata queries

## 3. Common MCP SQL Operations

### Executing Queries
- Use `mcp_call_tool` with the appropriate query execution tool
- Pass SQL query as a parameter (usually in a `query` or `sql` field)
- Handle results returned by the MCP server
- Check for errors in the response

### Schema Inspection
- Use schema inspection tools to list tables, columns, indexes
- Query system tables/views through MCP
- Read database metadata resources with `mcp_read_resource`
- Understand relationships (foreign keys, constraints)

### Data Manipulation
- Use INSERT tools for adding data
- Use UPDATE tools for modifying existing data
- Use DELETE tools for removing data
- Always consider transaction safety
- Verify row counts affected

### Transaction Management
- Check if MCP server supports transaction tools
- Use BEGIN TRANSACTION before modifications
- COMMIT if successful, ROLLBACK on errors
- Ensure proper transaction isolation levels

## 4. Database-Specific Considerations

### PostgreSQL
- Use pg_catalog for system information
- Consider JSONB operations if available
- Check for array data types
- Use EXPLAIN ANALYZE for performance

### MySQL/MariaDB
- Use information_schema for metadata
- Consider storage engine differences (InnoDB vs MyISAM)
- Check for strict SQL modes
- Use EXPLAIN for query plans

### SQL Server
- Use sys schema for DMVs
- Check for stored procedures and functions
- Consider tempdb usage
- Use SET STATISTICS for performance

### SQLite
- Use sqlite_master for schema
- Consider PRAGMA commands
- Check for WAL mode
- Use EXPLAIN QUERY PLAN

## 5. Best Practices

### Query Construction
- Always use parameterized queries if MCP server supports it
- Avoid SQL injection by proper escaping
- Use LIMIT/OFFSET for large result sets
- Consider pagination for large datasets

### Error Handling
- Check error responses from MCP tools
- Handle connection errors gracefully
- Validate data types before operations
- Check constraint violations

### Performance
- Use appropriate indexes
- Avoid SELECT * when possible
- Use WHERE clauses to filter data
- Consider JOIN optimization
- Monitor query execution time

### Security
- Never expose credentials in queries
- Use least-privilege database accounts
- Validate user inputs
- Log sensitive operations appropriately
- Consider row-level security if available

## 6. Working with MCP Resources

Use `mcp_read_resource` to:
- Read database configuration
- Access table definitions
- Get connection information
- Retrieve query results stored as resources
- Access database documentation

Resource URIs typically follow patterns like:
- `<server_name>://table/schema.table_name`
- `<server_name>://query/result_id`
- `<server_name>://schema/information`

## 7. Testing and Validation

After SQL operations:
- Verify row counts match expectations
- Check data integrity constraints
- Test with sample data
- Validate relationships still work
- Roll back if unexpected results

## 8. Common Patterns

### Reading Data
1. List available tables using schema tools
2. Inspect table structure
3. Construct SELECT query
4. Execute via MCP tool
5. Process results

### Writing Data
1. Inspect target table structure
2. Validate data types and constraints
3. Begin transaction if supported
4. Execute INSERT/UPDATE/DELETE
5. Verify results
6. Commit or rollback

### Schema Changes
1. Backup schema if possible
2. Check dependencies
3. Plan migration steps
4. Execute DDL statements
5. Verify schema
6. Test affected applications

## 9. Troubleshooting

### Connection Issues
- Verify MCP server is running
- Check connection parameters
- Review MCP server logs
- Test with simple query first

### Query Errors
- Check SQL syntax
- Verify table/column names
- Validate data types
- Check constraints
- Review error messages from MCP

### Performance Issues
- Use EXPLAIN/execution plans
- Check for missing indexes
- Review query complexity
- Consider query optimization
- Check database server load

Ask the user to provide:
- Which database type they're working with
- What SQL operation they need to perform
- Available MCP server names (if known)
- Specific query or operation details
- Any error messages from MCP tools
