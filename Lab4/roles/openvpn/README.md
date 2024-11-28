# Role Name

OpenVPN role for Ubuntu

## Requirements

## Role Variables

defaults for openvpn-role

---

    easy_rsa_dir: "{{ ansible_env.HOME }}/easy-rsa"
    easy_rsa_binary: "/usr/share/easy-rsa"
    easy_rsa_vars_template: "templates/easy-rsa3.j2"

    easy_rsa_vars_EASYRSA_REQ_COUNTRY: "RU"
    easy_rsa_vars_EASYRSA_REQ_PROVINCE: "Saint-Petersburg"
    easy_rsa_vars_EASYRSA_REQ_CITY: "Saint-Petersburg"
    easy_rsa_vars_EASYRSA_REQ_ORG: "ITMO University"
    easy_rsa_vars_EASYRSA_REQ_EMAIL: "k60duongthihuelinh@gmail.com"
    easy_rsa_vars_EASYRSA_REQ_OU: "Community"
    easy_rsa_vars_EASYRSA_ALGO: "ec"
    easy_rsa_vars_EASYRSA_DIGEST: "sha512"

    openvpn_server_common_name: "server"
    openvpn_dir: "/etc/openvpn/server"
    openvpn_server_template: "templates/openvpn-server.conf.j2"

    openvpn_server_vars_ca: ca.crt
    openvpn_server_vars_cert: "{{ openvpn_server_common_name }}.crt"
    openvpn_server_vars_key: "{{ openvpn_server_common_name }}.key"
    openvpn_server_vars_port: 1194
    openvpn_server_vars_proto: udp
    openvpn_server_vars_tls_crypt: ta.key
    openvpn_server_vars_cipher: |
        cipher AES-256-GCM
        auth SHA256
    openvpn_server_vars_dh: none
    openvpn_server_vars_user: nobody
    openvpn_server_vars_group: nogroup

    openvpn_client_common_name: "client1"
    client_configs_dir: "{{ ansible_env.HOME }}/client-configs"
    openvpn_client_template: "templates/openvpn-client.conf.j2"

    openvpn_client_vars_remote: "{{ ansible_default_ipv4.address }}"
    openvpn_client_vars_remote_port: 1194
    openvpn_client_vars_proto: udp
    openvpn_client_vars_cipher: |
        cipher AES-256-GCM
        auth SHA256
    openvpn_client_vars_extra: |
        key-direction 1
        script-security 2
        up /etc/openvpn/update-resolv-conf
        down /etc/openvpn/update-resolv-conf

    ufw_before_path: "/etc/ufw/before.rules"
    ufw_before_content: "|
          *nat
          :POSTROUTING ACCEPT [0:0]
          -A POSTROUTING -s 0.0.0.0/0 -o 0.0.0.0/0 -j MASQUERADE
          COMMIT"
    ufw_forwarding_default_path: "/etc/default/ufw"
    ufw_forwarding_policy: "DEFAULT_FORWARD_POLICY=\"ACCEPT\""
    ufw_rule: "allow"
    ufw_port: "1194"
    ufw_proto: "udp"

## Dependencies

## Example Playbook

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: app
      roles:
         - openvpn

## License

BSD

## Author Information

Duong Thi Hue Linh, ITMO University
