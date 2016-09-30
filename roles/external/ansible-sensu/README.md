* - inventory/test dosyasındaki takip eden satır, client.conf'taki subscription alanını direkt olarak etkiliyor:

[ms]
localhost ansible_ssh_user=vagrant ansible_ssh_pass=vagrant ansible_ssh_port=2222

Köşeli parantez arasındaki "ms" grup deklarasyonu, aynı zamanda sensu client'ın subscription'ı oluyor. Bunu, templates/client.json.j2 dosyasındaki takip eden kod bloğu sağlıyor:

"subscriptions": 
      {{ group_names | to_nice_json }}

* - Rol, ansible-playbook -i inventory/test cm/all.yml komutu ile çalıştırıldığında config dosyalarının oluşturulduğu; ancak sadece sensu-client'ın başlatıldığı, ansible-playbook -i inventory/test cm/ms.yml ile çalıştırıldığında ise yine config dosyalarının oluşturulduğu; ancak bu kez client, api ve server uygulamalarının da çalıştığı test edildi. 

 -- ansible-playbook -i inventory/test cm/all.yml komutu çalıştırılırken, inventory/test içeriği:
[all]
localhost ansible_ssh_user=vagrant ansible_ssh_pass=vagrant ansible_ssh_port=2222

 -- ansible-playbook -i inventory/test cm/ms.yml komutu çalıştırılırken, inventory/test içeriği:
[ms]
localhost ansible_ssh_user=vagrant ansible_ssh_pass=vagrant ansible_ssh_port=2222

!!!Dolayısıyla bu rolü kullanmadan önce inventory klasörünün altında yapılacak çalışmalara dikkat edilmelidir. 