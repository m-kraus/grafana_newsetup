# vagrant testbench

`vagrant testbench` ist ein Framework zum erstellen von Ansible
Playbooks und Rollen, die zunächst auf Vagrant-VMs getestet werden
können.

Das gewünschte Playbook kann in `main.yml` erstellt werden. Diese
Datei wird automatisch beim Start der Vagrant-VMs mit `vagrant up`
geladen und provisioniert.

Sollte ein Proxy erforderlich sein, werden die Umgebungsvariablen
`http_proxy`, `https_proxy` und `no_proxy` ausgelesen und automatisch
in den Gastsystemen zugewiesen. Die Konfiguration wird in der
Ansible-Variable `proxy_config` gespeichert, falls man darauf im
eigenen Playbook zugreifen will.

Möchte man die Ansible-Playbooks erneut laufen lassen, genügt `vagrant
provision ctrl --provision-with=ansible_local`.

## Aufbau

Es werden 4 Vagrant-VMs gestartet: Auf `ctrl` wird Ansible in der
aktuellsten Version installiert und von dort aus werden die
Ansible-Playbooks und -Rollen aufgerufen. Das aktuelle Verzeichnis ist
in der VM unter `/vagrant` gemountet.

Die Maschinen `node1` `node2` und `node3` können in verschiedenster
Weise angesprochen werden. In diesem Beispiel sind alle 3 VMs in der
Gruppe `cluster` enthalten, `node1` ist Mitglied der Gruppe `master`
und `node2` sowie `node3` sind Mitglieder der Gruppe `nodes`.

Diese Einteilung ist unter `inventory/vagrant.ini` festgelegt. Dort
ist es auch möglich zusätzliche Variablen festzulegen.

In `inventory/hosts.ini` können Hosts definiert werden die bei einem
`ansible-playbook main.yml` angesprochen werden.

## Voraussetzungen

Auf der Wirtsmaschine muss Virtualbox und Vagrant installiert sein.
Die Konfiguration wurde erfolgreich unter Windows und MacOS getestet.
