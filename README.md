# homelab-ansible

Playbooks e inventario del homelab. AWX clona este repositorio en cada
ejecución: lo que no esté subido aquí, AWX no lo ve.

| Carpeta / fichero      | Qué es |
|------------------------|--------|
| `inventario/`          | Las máquinas (`maquinas`, sintaxis INI **sin** `.ini`) y cómo conectarse a ellas (sin contraseñas) |
| `preparar-maquinas/`   | Primer playbook: usuario, clave SSH y sudo para Ansible |
| `ansible.cfg`          | Configuración para validar en local |
| `.yamllint`            | Reglas de estilo YAML |

Cada playbook tiene su propio README con sus argumentos y etiquetas.

## Validar antes de subir

```bash
yamllint .
ansible-lint
ansible-inventory -i inventario --graph     # tienen que salir las dos máquinas
ansible-navigator run preparar-maquinas/playbook.yaml --syntax-check
```
