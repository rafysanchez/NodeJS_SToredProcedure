# NodeJS_SToredProcedure
NodeJS STored Procedure
* http://www.mysqltutorial.org/mysql-nodejs/call-stored-procedures/

For the demonstration, we create a new stored procedure filterTodo to query rows from the todos table based on the value of the completed field.


DELIMITER $$
 
CREATE PROCEDURE `filterTodo`(IN done BOOLEAN)
BEGIN
    SELECT * FROM todos WHERE completed = done;
END$$
 
DELIMITER ;
The stored procedure filterTodo returns the rows in the todos table based on the done argument. If the done argument is true, it returns all completed todos, otherwise, it returns the non-completed todos.

As you know, to call a stored procedure in MySQL, you use the CALL statement. For example, to call the filterTodo stored procedure, you execute the following statement:

1
CALL filterTodo(true);
The statement returns the following result set:


+----+--------------------------+-----------+
| id | title                    | completed |
+----+--------------------------+-----------+
|  4 | It should work perfectly |         1 |
+----+--------------------------+-----------+
1 row in set (0.00 sec)
 
Query OK, 0 rows affected (0.01 sec)
The following storedproc.js program calls the filterTodo stored procedure and returns the result set:


let mysql = require('mysql');
let config = require('./config.js');
 
let connection = mysql.createConnection(config);
 
let sql = `CALL filterTodo(?)`;
 
connection.query(sql, true, (error, results, fields) => {
  if (error) {
    return console.error(error.message);
  }
  console.log(results[0]);
});
 
connection.end();
Notice that the program uses the config.js module that stores the database’s information:


let config = {
  host    : 'localhost',
  user    : 'root',
  password: '',
  database: 'todoapp'
};
 
module.exports = config;
In the CALL statement, we used a placeholder (?) to pass data to the stored procedure.

When we called the query() method on the connection object, we pass the value of the done argument as the second argument of the query() method.

Let’s run the storedproc.js program.

> node storedproc.js
[ RowDataPacket { id: 4, title: 'It should work perfectly', completed: 1 } ]
The program displayed 1 row as expected.

In this tutorial, you have learned how to call a stored procedure in MySQL from a Node.js program.


