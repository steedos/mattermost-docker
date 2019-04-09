### 配置支持中文搜索
```bash
docker exec -it mattermostdocker_db_1 bash
sudo chown -R postgres:postgres /usr/local/share/postgresql/tsearch_data/
sudo -i -u postgres
psql mattermost mmuser -c 'CREATE EXTENSION zhparser;'
psql mattermost mmuser -c 'CREATE TEXT SEARCH CONFIGURATION simple_zh_cfg (PARSER = zhparser);'
psql mattermost mmuser -c 'ALTER TEXT SEARCH CONFIGURATION simple_zh_cfg ADD MAPPING FOR n,v,a,i,e,l WITH simple;'
```
- 将/srv/mattermost/volumes/db/var/lib/postgresql/data/postgresql.conf中default_text_search_config 的值更改为 simple_zh_cfg 然后重启