Configuring Postfix to Relay through Mailgun
============================================

```
mailgun_username: CONFIGURE
mailgun_password: CONFIGURE
root_forward: CONFIGURE_EMAIL
---
- name: install postfix
  apt: name={{ item }} state=present
  with_items:
    - postfix
    - mailutils

- name: Include Mailgun configuration in main.conf
  lineinfile:
    dest: /etc/postfix/main.cf
    line: "{{ item.line }}"
    regexp: "{{ item.regexp }}"
  with_items:
    - { regexp: "^relayhost", line: "relayhost = [smtp.mailgun.org]:587" }
    - { regexp: "^smtp_tls_security_level", line: "smtp_tls_security_level = encrypt" }
    - { regexp: "^smtp_tls_note_starttls_offer", line: "smtp_tls_note_starttls_offer = yes" }
    - { regexp: "^smtp_sasl_auth_enable", line: "smtp_sasl_auth_enable = yes" }
    - { regexp: "^smtp_sasl_password_maps", line: "smtp_sasl_password_maps = static:{{ mailgun_username }}:{{ mailgun_password }}" }
    - { regexp: "^smtp_sasl_security_options", line: "smtp_sasl_security_options = noanonymous" }
  notify:
    - restart postfix

- name: Ensure Postfix has started and will start on reboot
  service: name=postfix state=started enabled=yes

- name: Set root's email forward
  lineinfile:
    dest: '/root/.forward'
    create: yes
    line: "{{ root_forward }}"
  when:
    - root_forward is defined
```
