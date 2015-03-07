# ansible playbookでswapファイル作成

ansible playbookを利用してswapファイル作成するための方法は、[Add swap module #5241](https://github.com/ansible/ansible/issues/5241#issuecomment-31438159)に詳しい。
swap moduleを追加しようとしてリジェクトを食らったようだ。

```yaml
---
- name: exists of swap file?
  shell: 'test -f /tmp/swap.img'
  register: exists_of_swap_file
  when: ansible_swaptotal_mb < 1

- name: create swap
  shell: nice dd if=/dev/zero of=/tmp/swap.img bs=1M count=2048
  when: exists_of_swap_file|failed and ansible_swaptotal_mb < 1

- name: make swap
  shell: mkswap /tmp/swap.img
  when: exists_of_swap_file|failed and ansible_swaptotal_mb < 1

- name: swap on
  shell: swapon /tmp/swap.img
  when: exists_of_swap_file|failed and ansible_swaptotal_mb < 1

- name: set swappiness
  sysctl: name=vm.swappiness
          value=0
```
