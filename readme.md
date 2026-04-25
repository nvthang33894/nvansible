👉 Tách theo environment / customer

Cấu trúc nên dùng:
ansible/
├── inventories/
│   ├── dev/
│   │   └── hosts.ini
│   ├── prod/
│   │   └── hosts.ini
│   ├── customer-a/
│   │   └── hosts.ini
│   └── customer-b/
│       └── hosts.ini
├── group_vars/
├── host_vars/
├── roles/
└── playbook.yml



3. Cách chạy

Ví dụ:

ansible-playbook -i ansible/inventories/customer-a/hosts.ini ansible/playbook.yml

👉 Không cần sửa file
→ chỉ đổi inventory path

4. Nếu nhiều customer hơn nữa

👉 Đừng hardcode trong playbook

Dùng biến:

vars:
  app_name: "{{ customer_name }}"

Hoặc:

ansible-playbook ... -e "customer=customer-a"
5. Trong GitHub Actions

Bạn nên làm kiểu:

- name: Run Ansible
  run: |
    ansible-playbook -i ansible/inventories/${{ inputs.env }}/hosts.ini ansible/playbook.yml

👉 Sau này:

chọn env=customer-a
hoặc env=prod
6. Tư duy quan trọng

Sai lầm phổ biến:

“Càng nhiều khách → càng sửa file”


7. Kết luận
✅ Bạn có thể chỉ sửa inventory.ini → chạy được
❌ Nhưng không nên làm lâu dài
✅ Nên:
tách inventory theo env/customer
giữ playbook chung
dùng biến để customize
Cách đúng:

Càng nhiều khách → càng tách config, không sửa logic
