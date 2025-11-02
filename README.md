# ansible - Working with ansible

## 1. Prerequis

### 1.1. packets complémentaires

```SHELL
sudo apt install \
    vim \
    python3 \
    python3-pip \
    pipx \
    curl \
    wget \
    tree \
    git
```

## 2. Installation de Ansible

## 2.1. RTFM

[Le guide d'installation officiel](https://docs.ansible.com/ansible/latest/installation_guide/index.html)

## 2.2. Choix

J'ai choisi la méthode avec `pipx` :

### 2.1.1. Installation d'Ansible

Utilisez `pipx` dans votre environnement pour installer le package Ansible complet :

```SHELL
pipx install --include-deps ansible
```

Vous pouvez installer le package minimal `ansible-core` :

```SHELL
pipx install ansible-core
```

Vous pouvez également installer une version spécifique d'`ansible-core` :

```SHELL
pipx install ansible-core==2.12.3
```

### 2.2.2. Mise à jour d'Ansible (pour plus tard...)

Pour mettre à jour une installation Ansible existante vers la dernière version publiée :

```SHELL
pipx upgrade --include-injected ansible
```

### 2.2.3. Installation de dépendances Python supplémentaires

Pour installer les dépendances Python supplémentaires nécessaires, prenons l'exemple de l'installation du package Python argcomplete (auto completion en CLI ; très util):

```SHELL
pipx inject ansible argcomplete
```

Ajoutez l'option `--include-apps` pour rendre les applications de la dépendance Python supplémentaire accessibles via votre variable d'environnement PATH. Vous pourrez ainsi exécuter des commandes pour ces applications depuis le terminal.

```SHELL
pipx inject --include-apps ansible argcomplete
```

Si vous devez installer des dépendances à partir d'un fichier requirements.txt, par exemple lors de l'installation de la collection Azure, vous pouvez utiliser runpip.

```SHELL
pipx runpip ansible install -r ~/.ansible/collections/ansible_collections/azure/azcollection/requirements.txt
```

## 3. Structure du projet

J'ai choisi de placer chaque fichier d'inventaire, avec ses variables de groupe/variables d'hôte, dans un répertoire distinct. 

Ceci est particulièrement utile lorsque les  variables de groupe et variables d'hôte présentent peu de points communs selon 
les environnements (nom des utilisateur et leur mot de passe, nom des bases de données, etc.).

```SHELL
.
├── ansible.cfg
├── inventories
│   ├── dev             # development environment
│   │   ├── group_vars  # - folder where group variables to particular groups
│   │   ├── hosts.yaml  # - inventory file of servers
│   │   └── host_vars   # - folder where group variables to particular hosts
│   └── prd             # production environment
│       ├── group_vars  # - folder where group variables to particular groups
│       ├── hosts.yaml  # - inventory file of servers
│       └── host_vars   # - folder where group variables to particular hosts
├── playbooks           # directory where grouping any playbooks
├── library             # directory where grouping any custom modules (optional)
├── module_utils        # directory where grouping any custom module_utils to support modules (optional)
├── filter_plugins      # directory where grouping any custom filter plugins (optional)
└── roles               # directory where grouping all roles
```