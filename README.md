1. ���������� ��������� playbook �� ��������� �� test.yml, ������������ ����� �������� ����� ���� some_fact ��� ���������� ����� ��� ���������� playbook'a.

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


2. ������� ���� � ����������� (group_vars) � ������� ������� ��������� � ������ ������ �������� � ��������� ��� �� 'all default fact'.
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




3. �������������� �������������� (������������ docker) ��� �������� ����������� ��������� ��� ���������� ���������� ���������.

���������


4. ��������� ������ playbook �� ��������� �� prod.yml. ������������ ���������� �������� some_fact ��� ������� �� managed host.
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



5. �������� ����� � group_vars ������ �� ����� ������ ���, ����� ��� some_fact ���������� ��������� ��������: ��� deb - 'deb default fact', ��� el - 'el default fact'.

������

6. ��������� ������ playbook �� ��������� prod.yml. ���������, ��� �������� ���������� �������� ��� ���� ������.

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



7. ��� ������ ansible-vault ���������� ����� � group_vars/deb � group_vars/el � ������� netology.
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

8. ��������� playbook �� ��������� prod.yml. ��� ������� ansible ������ ��������� � ��� ������. ��������� � �����������������.

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


9. ���������� ��� ������ ansible-doc ������ �������� ��� �����������. �������� ���������� ��� ������ �� control node.

������ local


10. � prod.yml �������� ����� ������ ������ � ������ local, � ��� ���������� localhost � ����������� ����� �����������.

������

11. ��������� playbook �� ��������� prod.yml. ��� ������� ansible ������ ��������� � ��� ������. ��������� ��� ����� some_fact ��� ������� �� ������ ���������� �� ������ group_vars.


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

12. ��������� README.md �������� �� �������. �������� git push � ����� master. � ������ ��������� ������ �� ��� �������� ����������� � ��������� playbook � ����������� README.md.

��������