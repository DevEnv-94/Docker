


```bash
user@server:~/fat_free_crm$ cat .dockerignore
.git/ #direcotry
.*.yml #all hidden file with yml extension 
./public/avatars/**/*.gif # all gif files in public/avatars/** sub directories
config/*.yml # all yml files in config direcrtories 
!config/settings.default.yml            # 
!config/database.sqlite.yml             # except this files 
!config/database.postgres.docker.yml/   #
```