
*Data de Criação:* 2025-01-12
*Última Atualização:* 2025-01-12

## Descrição
[Arquivo padrão para facilitar atualização do arquivo 50-server.cnf]

## Contexto
[Será usado para alterar o arquivo 50-server.cnf]

## Código SQL
```
[################ IXCSoft ################
############### CONFIG NOME ###############
### EMPRESA ###
#RAM: 
#SWAP: 
#Processador: 
#Disco: 
#########################################

[server]

[client]

[mysqld]

#General 
datadir                              = /var/lib/mysql
tmpdir                               = /tmp
user                                 = mysql
pid-file                             = /run/mysqld/mysqld.pid
socket                               = /run/mysqld/mysqld.sock
lc-messages-dir                      = /usr/share/mysql
bind-address                         = 0.0.0.0
sql_mode                             = ""

#Config Sort and Group
sort_buffer_size                     = 2M
innodb_sort_buffer_size              = 2M
read_rnd_buffer_size                 = 4M
max_sort_length                      = 1K
max_length_for_sort_data             = 10K
join_buffer_size                     = 1M
group_concat_max_len                 = 8096

#Config Tables
table_open_cache                     = 2000
table_definition_cache               = 1000

#Config Conections
max_allowed_packet                   = 512M
net_buffer_length                    = 16384
max_connections                      = 300
max_connect_errors                   = 1000
skip_name_resolve                    = 1
#max_user_connections                = 3000
#interactive_timeout                 = 600

#Config Cache
thread_cache_size                    = 11
query_cache_type                     = 0
query_cache_size                     = 0

#Config Tmp Tables
tmp_table_size                       = 16M
max_heap_table_size                  = 16M

#Config innodb
innodb_data_home_dir                 = /var/lib/mysql
innodb_buffer_pool_size              = 10G
innodb_flush_log_at_trx_commit       = 1
innodb_doublewrite                   = 1
innodb_flush_method                  = O_DIRECT
innodb_fast_shutdown                 = 1
innodb_buffer_pool_dump_at_shutdown  = 0
innodb_buffer_pool_load_at_startup   = 0
innodb_print_all_deadlocks           = 1
innodb_status_output_locks           = 1

#Config InnoDB Redo Logs
innodb_log_file_size                 = 1G

#Disco
innodb_io_capacity                   = 1000
innodb_io_capacity_max               = 2000

#Config Logs
log_output                           = FILE
log_error                            = /var/log/mysql/error.log
general_log                          = 0
general_log_file                     = /var/log/mysql/general.log
slow_query_log                       = 0
slow_query_log_file                  = /var/log/mysql/slow_query.log
log_slow_admin_statements            = 1
log_slow_slave_statements            = 0
long_query_time                      = 30
slow_launch_time                     = 15
log_queries_not_using_indexes        = 0

#Config Replication
#server_id                           = 1
#binlog_format                       = ROW
#binlog_row_image                    = MINIMAL
#sync-master-info                    = 1
#log_bin                             = db01-bin
#relay_log                           = db01-relay-bin
#report_host                         = db01
#expire_logs_days                    = 5
#auto_increment_increment            = 2 
#auto_increment_offset               = 1 
#log_bin_trust_function_creators     = 1

#Performance Schema
performance_schema                   = 0
# performance-schema-instrument        = 'memory/%=ON'

character-set-server                 = utf8mb4
collation-server                     = utf8mb4_general_ci

[embedded]

[mariadb]

[mariadb-10.11]

