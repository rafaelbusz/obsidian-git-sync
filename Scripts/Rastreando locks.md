Identificar e monitorar transações bloqueadas no banco de dados, além de utilizar o log de queries lentas para auxiliar na identificação de problemas.
Ao executar os comandos abaixo se atentar ao tempo/tamanho da tabela mysql.slow_log.

```sql
-- Cria tabela que salva os locks
CREATE TABLE blocked_transactions_log (
    trx_mysql_thread_id_blocked BIGINT NOT NULL,
    db_blocked VARCHAR(255),
    time_blocked INT,
    trx_query_blocked TEXT,
    trx_operation_state_blocked VARCHAR(255),
    trx_mysql_thread_id_blocker BIGINT NOT NULL,
    db_blocker VARCHAR(255),
    time_blocker INT,
    trx_query_blocker TEXT,
    trx_operation_state_blocker VARCHAR(255),
    log_timestamp TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Ativa event
SET GLOBAL event_scheduler = ON;

-- Create event
CREATE DEFINER=`root`@`localhost` EVENT `log_blocked_transactions` ON SCHEDULE EVERY 49 SECOND STARTS '2024-08-09 15:10:31' ON COMPLETION NOT PRESERVE ENABLE DO INSERT INTO blocked_transactions_log (
    trx_mysql_thread_id_blocked, db_blocked, time_blocked, trx_query_blocked, trx_operation_state_blocked,
    trx_mysql_thread_id_blocker, db_blocker, time_blocker, trx_query_blocker, trx_operation_state_blocker
)
SELECT
    TRX_BLOCKED.TRX_MYSQL_THREAD_ID AS TRX_MYSQL_THREAD_ID_BLOCKED,
    PROCESSLIST_BLOCKED.DB AS DB_BLOCKED,
    PROCESSLIST_BLOCKED.TIME AS TIME_BLOCKED,
    TRX_BLOCKED.trx_query AS trx_query_BLOCKED,
    TRX_BLOCKED.trx_operation_state AS trx_operation_state_BLOCKED,
    TRX_BLOCKER.TRX_MYSQL_THREAD_ID AS TRX_MYSQL_THREAD_ID_BLOCKER,
    PROCESSLIST_BLOCKER.DB AS DB_BLOCKER,
    PROCESSLIST_BLOCKER.TIME AS TIME_BLOCKER,
    TRX_BLOCKER.trx_query AS trx_query_BLOCKER,
    TRX_BLOCKER.trx_operation_state AS trx_operation_state_BLOCKER
FROM information_schema.INNODB_TRX AS TRX_BLOCKED
    INNER JOIN information_schema.PROCESSLIST AS PROCESSLIST_BLOCKED ON PROCESSLIST_BLOCKED.ID = TRX_BLOCKED.TRX_MYSQL_THREAD_ID
    INNER JOIN information_schema.INNODB_LOCK_WAITS ON INNODB_LOCK_WAITS.REQUESTING_TRX_ID = TRX_BLOCKED.trx_id
    INNER JOIN information_schema.INNODB_TRX AS TRX_BLOCKER ON TRX_BLOCKER.trx_id = INNODB_LOCK_WAITS.BLOCKING_TRX_ID
    INNER JOIN information_schema.PROCESSLIST AS PROCESSLIST_BLOCKER ON PROCESSLIST_BLOCKER.ID = TRX_BLOCKER.TRX_MYSQL_THREAD_ID

-- Select locks
SELECT * FROM blocked_transactions_log;

-- Também ativar o slow_log para identificar qual a conexão posteriormente
SET GLOBAL slow_query_log = 1;
SET GLOBAL long_query_time = 0;
SET GLOBAL log_output = 'table';

truncate mysql.slow_log;

SELECT * FROM mysql.slow_log;
