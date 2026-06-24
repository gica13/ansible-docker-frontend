# Docker-Ansible-Frontend

## Projekt 2 – Automatisation mit Ansible

### Projektbeschreibung

Dieses Projekt automatisiert die Bereitstellung einer Docker-basierten Webanwendung mit Ansible.

Ein Ansible-Controller verwaltet einen Debian Managed Host über SSH mittels Public-Key-Authentifizierung. Über ein Playbook wird Docker automatisch installiert und ein Nginx-Webserver als Container gestartet. Die Webseite wird anschliessend über HTTP bereitgestellt.

Die gesamte Infrastruktur ist reproduzierbar und kann jederzeit durch erneutes Ausführen des Ansible-Playbooks neu erstellt werden.

---

## Infrastruktur

| VM  | Hostname           | Rolle                   | IP-Adresse    |
| --- | ------------------ | ----------------------- | ------------- |
| VM1 | ansible-controller | Ansible Controller      | 192.168.200.5 |
| VM2 | docker-server      | Docker Host / Webserver | 192.168.200.6 |

---

## Netzwerk

```text
192.168.200.0/24

+----------------------+
| ansible-controller   |
| 192.168.200.5        |
+----------+-----------+
           |
           | SSH (Public Key)
           |
+----------+-----------+
| docker-server        |
| 192.168.200.6        |
+----------------------+
           |
           | HTTP :80
           |
        Browser
```

---

## Verwendete Technologien

* Debian 13
* Ansible
* Docker
* Nginx
* SSH Public Key Authentication
* Git
* GitHub

---

## Projektstruktur

```text
projekt2/
├── inventory.ini
├── playbook.yml
├── group_vars/
│   └── all.yml
└── roles/
    └── docker_frontend/
        ├── tasks/
        │   └── main.yml
        └── templates/
            └── index.html.j2
```
---


## Zugriff auf die Systeme

### Ansible Controller

Verbindung zum Controller Host:

```bash
ssh root@DyDNS-Hostname
```

### Verbindung auf Docker Host über den Controller

Vom Ansible Controller aus kann auf den Docker Host zugegriffen werden:

```bash
ssh root@192.168.200.6
```

### Webanwendung

Die bereitgestellte Webanwendung ist über HTTP erreichbar:

```text
http://192.168.200.6 oder curl http://192.168.200.6
```
---

## Inventory

Datei: `inventory.ini`

```ini
[webservers]
dockerhost ansible_host=192.168.200.6 ansible_user=root
```

---

## Variablen

Datei: `group_vars/all.yml`

```yaml
frontend_port: 80
container_name: frontend

project_name: "Projekt 2"
subtitle: "Docker Deployment mit Ansible"
```

---

## Template

Datei: `roles/docker_frontend/templates/index.html.j2`

```html
<html>
<body>
<h1>{{ project_name }}</h1>
<h2>{{ subtitle }}</h2>

<p>Frontend läuft erfolgreich.</p>

<p>Host: {{ inventory_hostname }}</p>
</body>
</html>
```

---

## Deployment

### Verbindung testen

```bash
ansible all -i inventory.ini -m ping
```

Beispielausgabe:

```text
dockerhost | SUCCESS => {
    "ping": "pong"
}
```

### Playbook ausführen

```bash
ansible-playbook -i inventory.ini playbook.yml
```

---

## Docker

Container prüfen:

```bash
docker ps
```

Beispielausgabe:

```text
CONTAINER ID   IMAGE          STATUS
xxxxxxxxxxxx   nginx:latest   Up
```

---

## HTTP-Test

Webseite über HTTP abrufen:

```bash
curl http://192.168.200.6
```

Beispielausgabe:

```html
<html>
<body>
<h1>Projekt 2</h1>
<h2>Docker Deployment mit Ansible</h2>

<p>Frontend läuft erfolgreich.</p>

<p>Host: dockerhost</p>
</body>
</html>
```

---

## Erfüllte Anforderungen

* [x] Ansible Controller Host
* [x] Managed Host
* [x] SSH Public-Key Authentication
* [x] Inventory
* [x] Variables
* [x] Templates
* [x] Playbook
* [x] Roles
* [x] Docker Deployment
* [x] HTTP Frontend
* [x] Git Repository

---

## Fazit

Mit diesem Projekt wurde eine vollständige Automatisierung einer Docker-basierten Webanwendung mittels Ansible umgesetzt. Die Installation von Docker, die Bereitstellung eines Nginx-Containers sowie die Konfiguration einer Webseite erfolgen vollständig automatisiert über ein Ansible Playbook. Durch die Verwendung von Roles, Variablen und Templates ist die Lösung modular, wiederverwendbar und einfach erweiterbar.

