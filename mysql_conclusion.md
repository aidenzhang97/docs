# notes

* mysql architecture

* run script or batch file

    mysql < batch-file | more
    mysql < batch-file > mysql.out
    mysql -h host -u user < batch-file

    senior:

        * run a query repeatly(every day or ervery week),makeing a script and enable it.
        * generate new queries form existing ones than are smilar by copying and editing script files.
        * for multiple-line statements or multiple-statements sequences.
        * share script with other people.
        * for not allow interactive use.

* mysql function

   | * build-in function

        name parsing rule:
            count()
            count ()
            if has whitespace after build-in function name, it can be permitted as an identifier depend on ignore_space sql mode and context;
            if no whitespace between build-in function name and ( parenthesis, it will be parsed as function name.

    | * loadable function
        fuctions can be loaded in runtime;like mysql enterprise edition include functions thar perform encryption opeartions based on the openSSL library;

    | * stored function
        function created by statement `create funtion`;
        demo:
        create table user(id int not null auto increment,birth date not null,primary key(id));
        insert into user(birth) values("1997-03-15");
        DILIMITER $$
        create function age(d DATE) returns int deterministic
        begin
            declare curD date;
            select current_date() into curD;
            return year(curD)-year(d);
        end $$

* user define variables

    SET @var_name = expre [,@var_name = expr]...

* mysql event

* mysql query attributes

    need component query_attributes

* mysql comments

    from a # character to the end of the line.

    from a -- sequence to the end of the line.

    from a `/*` sequence to the following `*/` sequence.

* mysql charset and collations

    change charset: set NAMES 'utf-8'

* mysql time build-in function

    current_date()
    current_time()
    current_timestamp()

* mysql data type:

    | numeric data type:

        * int
        * smallint
        * tinyint
        * mediumint
        * bigint
        * float
        * double or real
        * decimal
        * bit

    | date and time data types

        * date
        * time
        * datetime
        * timestamp
        * year

    | string data types

        * char
        * varchar
        * tinytext
        * text
        * mediumtext
        * longtext
        * binary
        * varbinary
        * blob
        * tinyblob
        * mediumblob
        * longblob
        * enum
        * set

    | spatial data types(geographic features or geospatial feature)

        * geometry
        * point
        * linestring
        * polygon

        * multipoint
        * multilinestring
        * multipolygon
        * getometrycollection

    | json data type

        UPDATE func: JSON_SET() JSON_REPLACE() JSON_REMOVE()

* mysql tablespace

    | file-per-table tablespace

        stored in a .ibd file
        be controlled by mysql setting innodb_file_per_table
    | general tablespace

        create tablespace ...
        stored in a .ibd file
    | system tablespace

        stored in the ibdata*file
        be controlled by mysql setting innodb_file_per_table

* mysql statements

    | data definition statements

        * atomic DDL(data defination language) support

            logs file: mysql.innodb_ddl_log

            support both table and non-table DDL statements;
                *table-related DDL statements:
                    CREATE,ALERT,DROP for databases,tablespaces,tables and indexes, and TRUNCATE TABLE statement
                *non-talbe DDL statements:
                    CREATE,DROP statements and if applicable,ALERT statements for stored programs,triggers,views,and loadable functions.
                    Account management statements:CREATE,ALERT,DROP and if applicable,RENAME statements for users and roles,as well as GRANT and REVOKE statements.
            not support DDL statements as follows:
                * table-related DDL statements that involve a storage engine other than InnoDB.
                * install plugin and uninstall plugin statement.
                * install component and uninstall component statements.
                * create server,alter server and drop server statements.

        * ALTER DATABASE Statement

            ALTER {DATABASE | SCHEMA} [db_name]
                alter_option ...

            alter_option: {
                [DEFAULT] CHARACTER SET [=] charset_name
            | [DEFAULT] COLLATE [=] collation_name
            | [DEFAULT] ENCRYPTION [=] {'Y' | 'N'}
            | READ ONLY [=] {DEFAULT | 0 | 1}
            }
        * ALTER EVENT Statement

            ALTER
                [DEFINER = user]
                EVENT event_name
                [ON SCHEDULE schedule]
                [ON COMPLETION [NOT] PRESERVE]
                [RENAME TO new_event_name]
                [ENABLE | DISABLE | DISABLE ON SLAVE]
                [COMMENT 'string']
                [DO event_body]
        * ALTER FUNCTION Statement

            ALTER FUNCTION func_name [characteristic ...]

            characteristic: {
                COMMENT 'string'
            | LANGUAGE SQL
            | { CONTAINS SQL | NO SQL | READS SQL DATA | MODIFIES SQL DATA }
            | SQL SECURITY { DEFINER | INVOKER }
            }
        * ALTER INSTANCE Statement

            ALTER INSTANCE instance_action

            instance_action: {
            | {ENABLE|DISABLE} INNODB REDO_LOG
            | ROTATE INNODB MASTER KEY
            | ROTATE BINLOG MASTER KEY
            | RELOAD TLS
                [FOR CHANNEL {mysql_main | mysql_admin}]
                [NO ROLLBACK ON ERROR]
            | RELOAD KEYRING
            }

        * ALTER LOGFILE Statement

            ALTER LOGFILE GROUP logfile_group
                ADD UNDOFILE 'file_name'
                [INITIAL_SIZE [=] size]
                [WAIT]
                ENGINE [=] engine_name

        * ALTER PROCEDURE Statement

            ALTER PROCEDURE proc_name [characteristic ...]

            characteristic: {
                COMMENT 'string'
            | LANGUAGE SQL
            | { CONTAINS SQL | NO SQL | READS SQL DATA | MODIFIES SQL DATA }
            | SQL SECURITY { DEFINER | INVOKER }
            }

        * ALTER SERVER Statement

            ALTER SERVER  server_name
                OPTIONS (option [, option] ...)

        * ALTER TABLE Statement

            ALTER TABLE tbl_name
                [alter_option [, alter_option] ...]
                [partition_options]

            alter_option: {
                table_options
            | ADD [COLUMN] col_name column_definition
                    [FIRST | AFTER col_name]
            | ADD [COLUMN] (col_name column_definition,...)
            | ADD {INDEX | KEY} [index_name]
                    [index_type] (key_part,...) [index_option] ...
            | ADD {FULLTEXT | SPATIAL} [INDEX | KEY] [index_name]
                    (key_part,...) [index_option] ...
            | ADD [CONSTRAINT [symbol]] PRIMARY KEY
                    [index_type] (key_part,...)
                    [index_option] ...
            | ADD [CONSTRAINT [symbol]] UNIQUE [INDEX | KEY]
                    [index_name] [index_type] (key_part,...)
                    [index_option] ...
            | ADD [CONSTRAINT [symbol]] FOREIGN KEY
                    [index_name] (col_name,...)
                    reference_definition
            | ADD [CONSTRAINT [symbol]] CHECK (expr) [[NOT] ENFORCED]
            | DROP {CHECK | CONSTRAINT} symbol
            | ALTER {CHECK | CONSTRAINT} symbol [NOT] ENFORCED
            | ALGORITHM [=] {DEFAULT | INSTANT | INPLACE | COPY}
            | ALTER [COLUMN] col_name {
                    SET DEFAULT {literal | (expr)}
                | SET {VISIBLE | INVISIBLE}
                | DROP DEFAULT
                }
            | ALTER INDEX index_name {VISIBLE | INVISIBLE}
            | CHANGE [COLUMN] old_col_name new_col_name column_definition
                    [FIRST | AFTER col_name]
            | [DEFAULT] CHARACTER SET [=] charset_name [COLLATE [=] collation_name]
            | CONVERT TO CHARACTER SET charset_name [COLLATE collation_name]
            | {DISABLE | ENABLE} KEYS
            | {DISCARD | IMPORT} TABLESPACE
            | DROP [COLUMN] col_name
            | DROP {INDEX | KEY} index_name
            | DROP PRIMARY KEY
            | DROP FOREIGN KEY fk_symbol
            | FORCE
            | LOCK [=] {DEFAULT | NONE | SHARED | EXCLUSIVE}
            | MODIFY [COLUMN] col_name column_definition
                    [FIRST | AFTER col_name]
            | ORDER BY col_name [, col_name] ...
            | RENAME COLUMN old_col_name TO new_col_name
            | RENAME {INDEX | KEY} old_index_name TO new_index_name
            | RENAME [TO | AS] new_tbl_name
            | {WITHOUT | WITH} VALIDATION
            }

            partition_options:
                partition_option [partition_option] ...

            partition_option: {
                ADD PARTITION (partition_definition)
            | DROP PARTITION partition_names
            | DISCARD PARTITION {partition_names | ALL} TABLESPACE
            | IMPORT PARTITION {partition_names | ALL} TABLESPACE
            | TRUNCATE PARTITION {partition_names | ALL}
            | COALESCE PARTITION number
            | REORGANIZE PARTITION partition_names INTO (partition_definitions)
            | EXCHANGE PARTITION partition_name WITH TABLE tbl_name [{WITH | WITHOUT} VALIDATION]
            | ANALYZE PARTITION {partition_names | ALL}
            | CHECK PARTITION {partition_names | ALL}
            | OPTIMIZE PARTITION {partition_names | ALL}
            | REBUILD PARTITION {partition_names | ALL}
            | REPAIR PARTITION {partition_names | ALL}
            | REMOVE PARTITIONING
            }

            key_part: {col_name [(length)] | (expr)} [ASC | DESC]

            index_type:
                USING {BTREE | HASH}

            index_option: {
                KEY_BLOCK_SIZE [=] value
            | index_type
            | WITH PARSER parser_name
            | COMMENT 'string'
            | {VISIBLE | INVISIBLE}
            }

            table_options:
                table_option [[,] table_option] ...

            table_option: {
                AUTOEXTEND_SIZE [=] value
            | AUTO_INCREMENT [=] value
            | AVG_ROW_LENGTH [=] value
            | [DEFAULT] CHARACTER SET [=] charset_name
            | CHECKSUM [=] {0 | 1}
            | [DEFAULT] COLLATE [=] collation_name
            | COMMENT [=] 'string'
            | COMPRESSION [=] {'ZLIB' | 'LZ4' | 'NONE'}
            | CONNECTION [=] 'connect_string'
            | {DATA | INDEX} DIRECTORY [=] 'absolute path to directory'
            | DELAY_KEY_WRITE [=] {0 | 1}
            | ENCRYPTION [=] {'Y' | 'N'}
            | ENGINE [=] engine_name
            | ENGINE_ATTRIBUTE [=] 'string'
            | INSERT_METHOD [=] { NO | FIRST | LAST }
            | KEY_BLOCK_SIZE [=] value
            | MAX_ROWS [=] value
            | MIN_ROWS [=] value
            | PACK_KEYS [=] {0 | 1 | DEFAULT}
            | PASSWORD [=] 'string'
            | ROW_FORMAT [=] {DEFAULT | DYNAMIC | FIXED | COMPRESSED | REDUNDANT | COMPACT}
            | SECONDARY_ENGINE_ATTRIBUTE [=] 'string'
            | STATS_AUTO_RECALC [=] {DEFAULT | 0 | 1}
            | STATS_PERSISTENT [=] {DEFAULT | 0 | 1}
            | STATS_SAMPLE_PAGES [=] value
            | TABLESPACE tablespace_name [STORAGE {DISK | MEMORY}]
            | UNION [=] (tbl_name[,tbl_name]...)
            }

            partition_options:
                (see CREATE TABLE options)

        * ALTER TABLESPACE Statement

            ALTER [UNDO] TABLESPACE tablespace_name
                NDB only:
                    {ADD | DROP} DATAFILE 'file_name'
                    [INITIAL_SIZE [=] size]
                    [WAIT]
                InnoDB and NDB:
                    [RENAME TO tablespace_name]
                InnoDB only:
                    [AUTOEXTEND_SIZE [=] 'value']
                    [SET {ACTIVE | INACTIVE}]
                    [ENCRYPTION [=] {'Y' | 'N'}]
                InnoDB and NDB:
                    [ENGINE [=] engine_name]
                Reserved for future use:
                    [ENGINE_ATTRIBUTE [=] 'string']

        * ALTER VIEW Statement

            ALTER
                [ALGORITHM = {UNDEFINED | MERGE | TEMPTABLE}]
                [DEFINER = user]
                [SQL SECURITY { DEFINER | INVOKER }]
                VIEW view_name [(column_list)]
                AS select_statement
                [WITH [CASCADED | LOCAL] CHECK OPTION]
        * CREATE DATABASE Statement

            CREATE {DATABASE | SCHEMA} [IF NOT EXISTS] db_name
                [create_option] ...

            create_option: [DEFAULT] {
                CHARACTER SET [=] charset_name
            | COLLATE [=] collation_name
            | ENCRYPTION [=] {'Y' | 'N'}
            }
        * CREATE EVENT Statement

            CREATE
                [DEFINER = user]
                EVENT
                [IF NOT EXISTS]
                event_name
                ON SCHEDULE schedule
                [ON COMPLETION [NOT] PRESERVE]
                [ENABLE | DISABLE | DISABLE ON SLAVE]
                [COMMENT 'string']
                DO event_body;

            schedule: {
                AT timestamp [+ INTERVAL interval] ...
            | EVERY interval
                [STARTS timestamp [+ INTERVAL interval] ...]
                [ENDS timestamp [+ INTERVAL interval] ...]
            }

            interval:
                quantity {YEAR | QUARTER | MONTH | DAY | HOUR | MINUTE |
                        WEEK | SECOND | YEAR_MONTH | DAY_HOUR | DAY_MINUTE |
                        DAY_SECOND | HOUR_MINUTE | HOUR_SECOND | MINUTE_SECOND}
        * CREATE FUNCTION Statement

            1.0 create stored function

                CREATE
                    [DEFINER = user]
                    PROCEDURE [IF NOT EXISTS] sp_name ([proc_parameter[,...]])
                    [characteristic ...] routine_body

                CREATE
                    [DEFINER = user]
                    FUNCTION [IF NOT EXISTS] sp_name ([func_parameter[,...]])
                    RETURNS type
                    [characteristic ...] routine_body

                proc_parameter:
                    [ IN | OUT | INOUT ] param_name type

                func_parameter:
                    param_name type

                type:
                    Any valid MySQL data type

                characteristic: {
                    COMMENT 'string'
                | LANGUAGE SQL
                | [NOT] DETERMINISTIC
                | { CONTAINS SQL | NO SQL | READS SQL DATA | MODIFIES SQL DATA }
                | SQL SECURITY { DEFINER | INVOKER }
                }

                routine_body:
                    Valid SQL routine statement

            2.0 create lodable function

                CREATE [AGGREGATE] FUNCTION [IF NOT EXISTS] function_name
                    RETURNS {STRING|INTEGER|REAL|DECIMAL}
                    SONAME shared_library_name
        * CREATE INDEX Statement

            CREATE [UNIQUE | FULLTEXT | SPATIAL] INDEX index_name
                [index_type]
                ON tbl_name (key_part,...)
                [index_option]
                [algorithm_option | lock_option] ...

            key_part: {col_name [(length)] | (expr)} [ASC | DESC]

            index_option: {
                KEY_BLOCK_SIZE [=] value
            | index_type
            | WITH PARSER parser_name
            | COMMENT 'string'
            | {VISIBLE | INVISIBLE}
            | ENGINE_ATTRIBUTE [=] 'string'
            | SECONDARY_ENGINE_ATTRIBUTE [=] 'string'
            }

            index_type:
                USING {BTREE | HASH}

            algorithm_option:
                ALGORITHM [=] {DEFAULT | INPLACE | COPY}

            lock_option:
                LOCK [=] {DEFAULT | NONE | SHARED | EXCLUSIVE}
        * CREATE LOGFILE GROUP Statement

            CREATE LOGFILE GROUP logfile_group
                ADD UNDOFILE 'undo_file'
                [INITIAL_SIZE [=] initial_size]
                [UNDO_BUFFER_SIZE [=] undo_buffer_size]
                [REDO_BUFFER_SIZE [=] redo_buffer_size]
                [NODEGROUP [=] nodegroup_id]
                [WAIT]
                [COMMENT [=] 'string']
                ENGINE [=] engine_name
        * CREATE PROCEDURE and CREATE FUNCTION Statements

            CREATE
                [DEFINER = user]
                PROCEDURE [IF NOT EXISTS] sp_name ([proc_parameter[,...]])
                [characteristic ...] routine_body

            CREATE
                [DEFINER = user]
                FUNCTION [IF NOT EXISTS] sp_name ([func_parameter[,...]])
                RETURNS type
                [characteristic ...] routine_body

            proc_parameter:
                [ IN | OUT | INOUT ] param_name type

            func_parameter:
                param_name type

            type:
                Any valid MySQL data type

            characteristic: {
                COMMENT 'string'
            | LANGUAGE SQL
            | [NOT] DETERMINISTIC
            | { CONTAINS SQL | NO SQL | READS SQL DATA | MODIFIES SQL DATA }
            | SQL SECURITY { DEFINER | INVOKER }
            }

            routine_body:
                Valid SQL routine statement

        * CREATE SERVER Statement

            CREATE SERVER server_name
                FOREIGN DATA WRAPPER wrapper_name
                OPTIONS (option [, option] ...)

            option: {
                HOST character-literal
            | DATABASE character-literal
            | USER character-literal
            | PASSWORD character-literal
            | SOCKET character-literal
            | OWNER character-literal
            | PORT numeric-literal
            }
        * CREATE SPATIAL REFERENCE SYSTEM Statement

            CREATE OR REPLACE SPATIAL REFERENCE SYSTEM
                srid srs_attribute ...

            CREATE SPATIAL REFERENCE SYSTEM
                [IF NOT EXISTS]
                srid srs_attribute ...

            srs_attribute: {
                NAME 'srs_name'
            | DEFINITION 'definition'
            | ORGANIZATION 'org_name' IDENTIFIED BY org_id
            | DESCRIPTION 'description'
            }

            srid, org_id: 32-bit unsigned integer

        * CREATE TABLE Statement

            CREATE [TEMPORARY] TABLE [IF NOT EXISTS] tbl_name
                (create_definition,...)
                [table_options]
                [partition_options]

            CREATE [TEMPORARY] TABLE [IF NOT EXISTS] tbl_name
                [(create_definition,...)]
                [table_options]
                [partition_options]
                [IGNORE | REPLACE]
                [AS] query_expression

            CREATE [TEMPORARY] TABLE [IF NOT EXISTS] tbl_name
                { LIKE old_tbl_name | (LIKE old_tbl_name) }

            create_definition: {
                col_name column_definition
            | {INDEX | KEY} [index_name] [index_type] (key_part,...)
                [index_option] ...
            | {FULLTEXT | SPATIAL} [INDEX | KEY] [index_name] (key_part,...)
                [index_option] ...
            | [CONSTRAINT [symbol]] PRIMARY KEY
                [index_type] (key_part,...)
                [index_option] ...
            | [CONSTRAINT [symbol]] UNIQUE [INDEX | KEY]
                [index_name] [index_type] (key_part,...)
                [index_option] ...
            | [CONSTRAINT [symbol]] FOREIGN KEY
                [index_name] (col_name,...)
                reference_definition
            | check_constraint_definition
            }

            column_definition: {
                data_type [NOT NULL | NULL] [DEFAULT {literal | (expr)} ]
                [VISIBLE | INVISIBLE]
                [AUTO_INCREMENT] [UNIQUE [KEY]] [[PRIMARY] KEY]
                [COMMENT 'string']
                [COLLATE collation_name]
                [COLUMN_FORMAT {FIXED | DYNAMIC | DEFAULT}]
                [ENGINE_ATTRIBUTE [=] 'string']
                [SECONDARY_ENGINE_ATTRIBUTE [=] 'string']
                [STORAGE {DISK | MEMORY}]
                [reference_definition]
                [check_constraint_definition]
            | data_type
                [COLLATE collation_name]
                [GENERATED ALWAYS] AS (expr)
                [VIRTUAL | STORED] [NOT NULL | NULL]
                [VISIBLE | INVISIBLE]
                [UNIQUE [KEY]] [[PRIMARY] KEY]
                [COMMENT 'string']
                [reference_definition]
                [check_constraint_definition]
            }

            data_type:
                (see Chapter 11, Data Types)

            key_part: {col_name [(length)] | (expr)} [ASC | DESC]

            index_type:
                USING {BTREE | HASH}

            index_option: {
                KEY_BLOCK_SIZE [=] value
            | index_type
            | WITH PARSER parser_name
            | COMMENT 'string'
            | {VISIBLE | INVISIBLE}
            |ENGINE_ATTRIBUTE [=] 'string'
            |SECONDARY_ENGINE_ATTRIBUTE [=] 'string'
            }

            check_constraint_definition:
                [CONSTRAINT [symbol]] CHECK (expr) [[NOT] ENFORCED]

            reference_definition:
                REFERENCES tbl_name (key_part,...)
                [MATCH FULL | MATCH PARTIAL | MATCH SIMPLE]
                [ON DELETE reference_option]
                [ON UPDATE reference_option]

            reference_option:
                RESTRICT | CASCADE | SET NULL | NO ACTION | SET DEFAULT

            table_options:
                table_option [[,] table_option] ...

            table_option: {
                AUTOEXTEND_SIZE [=] value
            | AUTO_INCREMENT [=] value
            | AVG_ROW_LENGTH [=] value
            | [DEFAULT] CHARACTER SET [=] charset_name
            | CHECKSUM [=] {0 | 1}
            | [DEFAULT] COLLATE [=] collation_name
            | COMMENT [=] 'string'
            | COMPRESSION [=] {'ZLIB' | 'LZ4' | 'NONE'}
            | CONNECTION [=] 'connect_string'
            | {DATA | INDEX} DIRECTORY [=] 'absolute path to directory'
            | DELAY_KEY_WRITE [=] {0 | 1}
            | ENCRYPTION [=] {'Y' | 'N'}
            | ENGINE [=] engine_name
            | ENGINE_ATTRIBUTE [=] 'string'
            | INSERT_METHOD [=] { NO | FIRST | LAST }
            | KEY_BLOCK_SIZE [=] value
            | MAX_ROWS [=] value
            | MIN_ROWS [=] value
            | PACK_KEYS [=] {0 | 1 | DEFAULT}
            | PASSWORD [=] 'string'
            | ROW_FORMAT [=] {DEFAULT | DYNAMIC | FIXED | COMPRESSED | REDUNDANT | COMPACT}
            | SECONDARY_ENGINE_ATTRIBUTE [=] 'string'
            | STATS_AUTO_RECALC [=] {DEFAULT | 0 | 1}
            | STATS_PERSISTENT [=] {DEFAULT | 0 | 1}
            | STATS_SAMPLE_PAGES [=] value
            | TABLESPACE tablespace_name [STORAGE {DISK | MEMORY}]
            | UNION [=] (tbl_name[,tbl_name]...)
            }

            partition_options:
                PARTITION BY
                    { [LINEAR] HASH(expr)
                    | [LINEAR] KEY [ALGORITHM={1 | 2}] (column_list)
                    | RANGE{(expr) | COLUMNS(column_list)}
                    | LIST{(expr) | COLUMNS(column_list)} }
                [PARTITIONS num]
                [SUBPARTITION BY
                    { [LINEAR] HASH(expr)
                    | [LINEAR] KEY [ALGORITHM={1 | 2}] (column_list) }
                [SUBPARTITIONS num]
                ]
                [(partition_definition [, partition_definition] ...)]

            partition_definition:
                PARTITION partition_name
                    [VALUES
                        {LESS THAN {(expr | value_list) | MAXVALUE}
                        |
                        IN (value_list)}]
                    [[STORAGE] ENGINE [=] engine_name]
                    [COMMENT [=] 'string' ]
                    [DATA DIRECTORY [=] 'data_dir']
                    [INDEX DIRECTORY [=] 'index_dir']
                    [MAX_ROWS [=] max_number_of_rows]
                    [MIN_ROWS [=] min_number_of_rows]
                    [TABLESPACE [=] tablespace_name]
                    [(subpartition_definition [, subpartition_definition] ...)]


            subpartition_definition:
                SUBPARTITION logical_name
                    [[STORAGE] ENGINE [=] engine_name]
                    [COMMENT [=] 'string' ]
                    [DATA DIRECTORY [=] 'data_dir']
                    [INDEX DIRECTORY [=] 'index_dir']
                    [MAX_ROWS [=] max_number_of_rows]
                    [MIN_ROWS [=] min_number_of_rows]
                    [TABLESPACE [=] tablespace_name]

            query_expression:
                SELECT ...   (Some valid select or union statement)

        * CREATE TABLESPACE Statement

            CREATE [UNDO] TABLESPACE tablespace_name

                InnoDB and NDB:
                    [ADD DATAFILE 'file_name']
                    [AUTOEXTEND_SIZE [=] value]

                InnoDB only:
                    [FILE_BLOCK_SIZE = value]
                    [ENCRYPTION [=] {'Y' | 'N'}]

                NDB only:
                    USE LOGFILE GROUP logfile_group
                    [EXTENT_SIZE [=] extent_size]
                    [INITIAL_SIZE [=] initial_size]
                    [MAX_SIZE [=] max_size]
                    [NODEGROUP [=] nodegroup_id]
                    [WAIT]
                    [COMMENT [=] 'string']

                InnoDB and NDB:
                    [ENGINE [=] engine_name]

                Reserved for future use:
                    [ENGINE_ATTRIBUTE [=] 'string']
        
        * CREATE TRIGGER Statement

            CREATE
                [DEFINER = user]
                TRIGGER [IF NOT EXISTS] trigger_name
                trigger_time trigger_event
                ON tbl_name FOR EACH ROW
                [trigger_order]
                trigger_body

            trigger_time: { BEFORE | AFTER }

            trigger_event: { INSERT | UPDATE | DELETE }

            trigger_order: { FOLLOWS | PRECEDES } other_trigger_name

        * CREATE VIEW Statement

            CREATE
                [OR REPLACE]
                [ALGORITHM = {UNDEFINED | MERGE | TEMPTABLE}]
                [DEFINER = user]
                [SQL SECURITY { DEFINER | INVOKER }]
                VIEW view_name [(column_list)]
                AS select_statement
                [WITH [CASCADED | LOCAL] CHECK OPTION]
        
        * DROP DATABASE Statement

            DROP {DATABASE|SCHEMA} [IF EXISTS] db_name

        * DROP EVENT Statemetn

            DROP EVENT [IF EXISTS] event_name

        * DROP FUNCTION and DROP PROCEDURE Statement

            DROP {PROCEDURE|FUNCTION} [IF EXISTS] sp_name
        
        * DROP INDEX Statement

            DROP INDEX index_name ON tbl_name
                [algorithm_option|lock_option]...
            
            algorithm_options:
                ALGORITHM [=] {DEFAULT | INPLACE | COPY}

            lock_option:
                LOCK [=] {DEFAULT | NONE | SHARED | EXCLUSIVE}
            
        * DROP LOGFILE GROUP Statement

            DROP LOGFILE GROUP logfile_group
                ENGINE [=] engine_name
        
        * DROP SERVER Statement

            DROP SERVER [IF EXISTS] server_name

        * DROP SPATAIL REFERENCE SYSTEM Statement

            DROP SPATAIL REFERENCE SYSTEM
                [IF EXISTS]
                srid
            
            srid: 32-bit unsigned integer

        * DROP TABLE Statement

            DROP [TEMPORARY] TABLE [IF EXISTS] 
                table_name [,table_name]...
                [RESTRIC|CASCADE]
            
        * DROP TABLESPACE Statement

            DROP [UNDO] TABLESPACE tablespace_name
                [ENGINE [=] engine_name]

        * DROP TRIGGER Statement

            DROP TRIGGER [IF EXISTS] [schema_name].trigger_name

        * DROP VIEW Statement

            DROP VIEW [IF EXISTS]
                view_name [,view_name]...
                [RESTRICT|CASCADE]
        
        * RENAME TABLE Statement

            RENAME TABLE
                table_name TO new_table_name
                [,table_name2 TO new_table_name2] ...

        * TRUNCATE TABLE Statement

            TRUNCATE [TABLE] table_name

    | data manupulation statements

        * CALL Statement

            CALL sp_name([parmeter[,...]])
            CALL sp_name[()]

        * DELETE Statement

            DELETE [LOW_PRIORITY] [QUICK] [IGNORE] FROM table_name [[AS] table_alias]
                [PARTITION (partition_name [,partition_name]...)]
                [WHERE where_condition]
                [ORDER BY...]
                [LIMIT row_count]
            
        * DO Statement

            DO expr[,expr]...

        * HANDLER Statement

            HANDLER table_name OPEN [[AS] alias]

            HANDLER table_name READ index_name {=|<=|>=|<|>}(value1,value2,...)
                [WHERE where_condition] [LIMIT...]
            HANDLER table_name READ index_name {FIRST | NEXT | PREV | LAST}
                [WHERE where_condition] [LIMIT...]
            HANDLER table_name READ {FIRST | NEXT}
                [WHERE where_condition] [LIMIT...]
            
            HANDER table_name CLOSE

        * IMPORT TABLE Statement

            IMPORT TABLE FROM sdi_file [,sdi_file]...

        * INSERT Statement

            INSERT [LOW_PRIORITY |DELAYED | HIGH_PRIORITY] [IGNORE]
                [INTO] tal_name
                [PARTITION (partition_name [,partition_name]...)]
                [(col_name [,col_name]...)]
                {{VALUES | VALUE}(value_list)[,(value_list)]...}
                [AS row_alias[(col_alias [,col_alias]...)]]
                [ON DUPLICATE KEY UPDATE assignment_list]

            INSERT [LOW_PRIORITY |DELAYED | HIGH_PRIORITY] [IGNORE]
                [INTO] tal_name
                [PARTITION (partition_name [,partition_name]...)]
                SET assignment_list
                [AS row_alias[(col_alias [,col_alias]...)]]
                [ON DUPLICATE KEY UPDATE assignment_list]
            
            INSERT [LOW_PRIORITY |DELAYED | HIGH_PRIORITY] [IGNORE]
                [INTO] tal_name
                [PARTITION (partition_name [,partition_name]...)]
                [(col_name [,col_name]...)]
                { SELECT ...
                  | TABLE table_name
                  | VALUES row_constructor_list
                }
                [ON DUPLICATE KEY UPDATE assignment_list]
            
            value:
                {expr | DEFAULT}

            value_list:
                value [, value] ...

            row_constructor_list:
                ROW(value_list)[, ROW(value_list)][, ...]

            assignment:
                col_name = 
                    value
                    | [row_alias.]col_name
                    | [tbl_name.]col_name
                    | [row_alias.]col_alias

            assignment_list:
                assignment [, assignment] ...

        * LOAD DATA Statement
        
            LOAD DATA
                [LOW_PRIORITY | CONCURRENT] [LOCAL]
                INFILE 'file_name'
                [REPLACE | IGNORE]
                INTO TABLE tbl_name
                [PARTITION (partition_name [, partition_name] ...)]
                [CHARACTER SET charset_name]
                [{FIELDS | COLUMNS}
                    [TERMINATED BY 'string']
                    [[OPTIONALLY] ENCLOSED BY 'char']
                    [ESCAPED BY 'char']
                ]
                [LINES
                    [STARTING BY 'string']
                    [TERMINATED BY 'string']
                ]
                [IGNORE number {LINES | ROWS}]
                [(col_name_or_user_var
                    [, col_name_or_user_var] ...)]
                [SET col_name={expr | DEFAULT}
                    [, col_name={expr | DEFAULT}] ...]

        * LOAD XML Statement

            LOAD XML
                [LOW_PRIORITY | CONCURRENT] [LOCAL]
                INFILE 'file_name'
                [REPLACE | IGNORE]
                INTO TABLE [db_name.]tbl_name
                [CHARACTER SET charset_name]
                [ROWS IDENTIFIED BY '<tagname>']
                [IGNORE number {LINES | ROWS}]
                [(field_name_or_user_var
                    [, field_name_or_user_var] ...)]
                [SET col_name={expr | DEFAULT}
                    [, col_name={expr | DEFAULT}] ...]
                
        * REPLACE Statement

            REPLACE [LOW_PRIORITY | DELAYED]
                [INTO] table_name
                [PARTITION (partition_name [,partition_name]...)]
                { {VALUES | VALUE}(value_list)[,value_list]...
                  |
                  VALUES row_constructor_list
                }

            REPLACE [LOW_PRIORITY | DELAYED]
                [INTO] table_name
                [PARTITION (partition_name [,partition_name]...)]
                SET assignment_list

            REPLACE [LOW_PRIORITY | DELAYED]
                [INTO] table_name
                [PARTITION (partition_name [,partition_name]...)]
                [(col_name)[,col_name]...]
                {SELECT ...| TABLE table_name}
            
            value:
                {expr | DEFALUT}

            value_list:
                value[,value]...
            
            row_constructor_list:
                ROW(value_list)[,ROW(value_list)][...]

            assignment:
                col_name = value
            
            assignment_list:
                assignment[,assignment]...

        * SELECT Statement

            SELECT
                [ALL | DISTINCT | DISTINCTROW]
                [HIGH_PRIORITY]
                [STRAIGHT_JOIN]
                [SQL_SMALL_RESULT] [SQL_BIG_RESULT] [SQL_BUFFER_RESULT]
                [SQL_NO_CACHE] [SQL_CALC_FOUND_ROWS]
                select_expr [,select_expr]...
                [into_option]
                [FROM table_reference [PARTITION partition_list]]
                [WHERE where_condition]
                [GROUP BY {col-name | expr | position},...[WITH ROLLUP]]
                [HAVING where_condition]
                [WINDOW window_name AS (window_spec)[,window_spec]...]
                [ORDER BY {col_name | expr | position} [ASC | DESC],...[WITH ROLLUP]]
                [LIMIT {[offset,] row_count | row_count OFFSET offset}]
                [into_option]
                [FOR {UPDATE | SHARE}
                   [OF tbl_name [,tbl_name]...]
                   [NOWAIR | SKIP LOCKED]
                   |[LOCK IN SHARE MODE] ]
                [into_option]

            into_option:{
                INTO OUTFILE 'file_name'
                 [CHARACTER SET charset_name]
                 export_options
                | INTO DUMPFILE 'file_name'
                | INTO var_name [,var_name]...
            }
        
        * TODO

    | transcational and locking statements
    | replication statements
    | prepared statements
    | compound statement syntax
    | database administration statements
    | utility statements

## demo table

    alomost have all features in mysql
    * all datatypes
    * can be grouped
    * can be sorted
    * can be partitioned
    * has foreign key relationship
    * can be indexed
    * unique conflict

    colulms:
    * id int primary key
    * group_id int index not null
    * order_id int not null
    * partition_id int not null
    * relshp_id int not null foreign key
    * desp varchar(300) null
    * f_val float not null
    * b_val binary not null
    * type enum('0','1') default '0'
    * json_val JSON null
    * creat_at datetime default create_time
    * update_at datetime current_date

    constraint:
    * primary key id
    * partition by partition_id, 10 partitions. partition_id in 0..9
    * create_at and update_at
    * index and unique group_id,order_id,group_id,partition_id

    table create:
    create table demo_parent(id int auto_increment primary key);
    create table demo(id int pri)