1. Попробуйте запустить playbook на окружении из test.yml, зафиксируйте какое значение имеет факт some_fact для указанного хоста при выполнении playbook'a.

```
ivan@ubt-tst:~/netology-ansible1$ ansible-playbook -i inventory/test.yml site.yml

PLAY [Print os facts] ***************************************************************************************************

TASK [Gathering Facts] **************************************************************************************************
ok: [localhost]

TASK [Print OS] *********************************************************************************************************
ok: [localhost] => {
    "msg": "Ubuntu"
}

TASK [Print fact] *******************************************************************************************************
ok: [localhost] => {
    "msg": 12
}

PLAY RECAP **************************************************************************************************************
localhost                  : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```
<img width="700" alt="2" src="https://github.com/Darkpunks/netologyProject/blob/main/second%20part/8.1..jpg">

2. Найдите файл с переменными (group_vars) в котором задаётся найденное в первом пункте значение и поменяйте его на 'all default fact'.
```
ivan@ubt-tst:~/netology-ansible1$ vi group_vars/all/examp.yml
```
```
ivan@ubt-tst:~/netology-ansible1$ ansible-playbook -i inventory/test.yml site.yml

PLAY [Print os facts] ***************************************************************************************************

TASK [Gathering Facts] **************************************************************************************************
ok: [localhost]

TASK [Print OS] *********************************************************************************************************
ok: [localhost] => {
    "msg": "Ubuntu"
}

TASK [Print fact] *******************************************************************************************************
ok: [localhost] => {
    "msg": "all default fact"
}

PLAY RECAP **************************************************************************************************************
localhost                  : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```




3. Воспользуйтесь подготовленным (используется docker) или создайте собственное окружение для проведения дальнейших испытаний.

Выполнено


4. Проведите запуск playbook на окружении из prod.yml. Зафиксируйте полученные значения some_fact для каждого из managed host.
```
ivan@ubt-tst:~/netology-ansible1$ ansible-playbook -i inventory/prod.yml site.yml

PLAY [Print os facts] ***************************************************************************************************

TASK [Gathering Facts] **************************************************************************************************
ok: [debian]
ok: [centos]

TASK [Print OS] *********************************************************************************************************
ok: [centos] => {
    "msg": "CentOS"
}
ok: [debian] => {
    "msg": "Debian"
}

TASK [Print fact] *******************************************************************************************************
ok: [centos] => {
    "msg": "el"
}
ok: [debian] => {
    "msg": "deb"
}

PLAY RECAP **************************************************************************************************************
centos                     : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
debian                     : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```



5. Добавьте факты в group_vars каждой из групп хостов так, чтобы для some_fact получились следующие значения: для deb - 'deb default fact', для el - 'el default fact'.

Сделал

6. Повторите запуск playbook на окружении prod.yml. Убедитесь, что выдаются корректные значения для всех хостов.

```
ivan@ubt-tst:~/netology-ansible1$ ansible-playbook -i inventory/prod.yml site.yml

PLAY [Print os facts] ***************************************************************************************************

TASK [Gathering Facts] **************************************************************************************************
ok: [debian]
ok: [centos]

TASK [Print OS] *********************************************************************************************************
ok: [centos] => {
    "msg": "CentOS"
}
ok: [debian] => {
    "msg": "Debian"
}

TASK [Print fact] *******************************************************************************************************
ok: [centos] => {
    "msg": "el default fact"
}
ok: [debian] => {
    "msg": "deb default fact"
}

PLAY RECAP **************************************************************************************************************
centos                     : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
debian                     : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```



7. При помощи ansible-vault зашифруйте факты в group_vars/deb и group_vars/el с паролем netology.
```
ivan@ubt-tst:~/netology-ansible1$ ansible-vault encrypt group_vars/deb/examp.yml
New Vault password:
Confirm New Vault password:
Encryption successful
ivan@ubt-tst:~/netology-ansible1$ ansible-vault encrypt group_vars/el/examp.yml
New Vault password:
Confirm New Vault password:
Encryption successful

```

8. Запустите playbook на окружении prod.yml. При запуске ansible должен запросить у вас пароль. Убедитесь в работоспособности.

```
ivan@ubt-tst:~/netology-ansible1$ ansible-playbook -i inventory/prod.yml site.yml --ask-vault-pass
Vault password:

PLAY [Print os facts] ***************************************************************************************************

TASK [Gathering Facts] **************************************************************************************************
ok: [debian]
ok: [centos]

TASK [Print OS] *********************************************************************************************************
ok: [centos] => {
    "msg": "CentOS"
}
ok: [debian] => {
    "msg": "Debian"
}

TASK [Print fact] *******************************************************************************************************
ok: [centos] => {
    "msg": "el default fact"
}
ok: [debian] => {
    "msg": "deb default fact"
}

PLAY RECAP **************************************************************************************************************
centos                     : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
debian                     : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```


9. Посмотрите при помощи ansible-doc список плагинов для подключения. Выберите подходящий для работы на control node.

Плагин local


10. В prod.yml добавьте новую группу хостов с именем local, в ней разместите localhost с необходимым типом подключения.

Сделал

11. Запустите playbook на окружении prod.yml. При запуске ansible должен запросить у вас пароль. Убедитесь что факты some_fact для каждого из хостов определены из верных group_vars.


```
ivan@ubt-tst:~/netology-ansible1$ ansible-playbook -i inventory/prod.yml site.yml --ask-vault-pass
Vault password:

PLAY [Print os facts] ***************************************************************************************************

TASK [Gathering Facts] **************************************************************************************************
ok: [localhost]
ok: [debian]
ok: [centos]

TASK [Print OS] *********************************************************************************************************
ok: [centos] => {
    "msg": "CentOS"
}
ok: [debian] => {
    "msg": "Debian"
}
ok: [localhost] => {
    "msg": "Ubuntu"
}

TASK [Print fact] *******************************************************************************************************
ok: [centos] => {
    "msg": "el default fact"
}
ok: [debian] => {
    "msg": "deb default fact"
}
ok: [localhost] => {
    "msg": "all default fact"
}

PLAY RECAP **************************************************************************************************************
centos                     : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
debian                     : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
localhost                  : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

```

12. Заполните README.md ответами на вопросы. Сделайте git push в ветку master. В ответе отправьте ссылку на ваш открытый репозиторий с изменённым playbook и заполненным README.md.

Заполнил
