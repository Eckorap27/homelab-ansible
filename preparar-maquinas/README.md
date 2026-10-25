# preparar-maquinas

Deja cada máquina lista para que Ansible la gestione:

- existe el usuario de Ansible (`ansible` por defecto),
- la clave pública de AWX está en su `authorized_keys`,
- puede hacer `sudo` sin contraseña.

Es el primer playbook que se lanza contra una máquina recién instalada. La
primera vez entra **con contraseña**; a partir de ahí, con la clave.

## Sistemas soportados

| Familia | Probado en      |
|---------|-----------------|
| RedHat  | Rocky Linux 9   |
| Debian  | Ubuntu 26.04    |

Solo usa módulos de `ansible.builtin`: funciona con la imagen por defecto de
AWX.

## Argumentos

| Argumento                         | Obligatorio | Por defecto | Qué es |
|-----------------------------------|-------------|-------------|--------|
| `preparacion.clave_publica`       | Sí          | —           | La clave **pública** SSH de AWX, en una línea |
| `preparacion.usuario`             | No          | `ansible`   | Usuario con el que entrará Ansible |
| `preparacion.sudo_sin_contrasena` | No          | `true`      | `false` quita el permiso si lo puso este playbook |

Ejemplo de extra vars:

```yaml
preparacion:
  clave_publica:      "ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAA... awx-homelab"
```

## Etiquetas

| Etiqueta | Qué ejecuta |
|----------|-------------|
| `acceso` | Usuario, clave autorizada y sudo |
| `prueba` | Solo las comprobaciones, sin cambiar nada |

## Cuándo marca `changed`

Solo cuando se crea el usuario, se añade la clave o cambia el permiso de sudo.
En una segunda ejecución tiene que salir `changed=0`.

## Lo que no hace (a propósito)

- No desactiva la entrada por contraseña en SSH: si la clave estuviera mal,
  te quedarías fuera. Será otro playbook, cuando el acceso por clave esté
  comprobado.
- No instala el servidor SSH: si no hay SSH, Ansible no puede ni entrar.
