# Seguridad en GIT

## Storyline

Trabajamos para una StartUp que se dedica al despliegue de cargadores para vehículos eléctricos en la ciudad. Recientemente nos ha dado acceso al repositorio GIT que utilizan los desarrolladores del Backend, nuestra tarea será verificar que utilizan GIT de una forma segura (https://github.com/TheMatrix97/CTF-SmartCharge). 
Tendréis que explorar a fondo el repositorio, buscando información que pueda comprometer la seguridad de la organización.

## Objetivos

- **Explorar el historial de commits, ramas y tags** para identificar commits sospechosos.

El workshop se puede hacer utilizando la interfaz gráfica de GitHub o por línea de comandos. Si estás familiarizado con GIT te recomiendo que los hagas por CLI y aproveches la oportunidad para repasar los comandos.

## Requisitos

- Tener instalado [Git](https://git-scm.com) en tu sistema.
- Conocimientos básicos de Git (clonar, hacer commits, trabajar con ramas, etc.).
- Acceso a una terminal o consola de comandos.


1. **Clona el repositorio**

   Abre una terminal y ejecuta:

   ```bash
   git clone https://github.com/TheMatrix97/CTF-SmartCharge
   cd CTF-SmartCharge
   ```

<details>
<summary>Hint</summary>

Encontrarás una propuesta de solución en el fichero [solution_ofuscated.md](./extra/solution_ofuscated.md). Utilízalo como último recurso ;)

```bash
cat extra/solution_ofuscated.md | base64 -d > extra/solution.md
```


</details>
