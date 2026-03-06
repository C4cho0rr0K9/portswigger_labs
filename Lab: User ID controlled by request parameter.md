# Reporte de Laboratorio: Horizontal Privilege Escalation (IDOR)

<img width="868" height="427" alt="Image" src="https://github.com/user-attachments/assets/e1115927-edbb-4c50-84ca-2303bfdec192" />

### Primer paso: Revisar el HTTP History en Burp Suite
Nuestra primera parada táctica es el historial de tráfico para entender cómo se comunica la aplicación.

<img width="1363" height="729" alt="Image" src="https://github.com/user-attachments/assets/523704fc-fc53-4774-95b2-8389b98961ab" />

### Interfaz del Laboratorio
Esta es la página principal sobre la que vamos a operar.

<img width="1344" height="606" alt="Image" src="https://github.com/user-attachments/assets/870fe923-1eaa-44a3-ad1b-1084f5865bb0" />

### Autenticación
Ingresamos las credenciales proporcionadas:
* **Username:** `wiener`
* **Password:** `peter`

<img width="1344" height="606" alt="Image" src="https://github.com/user-attachments/assets/0fb5a61d-b0c8-4432-afa0-4a2f98c59208" />

### Análisis del Tráfico Generado
Automáticamente, Burp Suite capturará el nuevo historial HTTP tras el login.

<img width="1363" height="140" alt="Image" src="https://github.com/user-attachments/assets/5e42cff4-f0e6-40be-9fb6-ecf0a72cc830" />

**¿Por qué nos interesa el ID #894 GET?**
* **Método GET:** Se utiliza específicamente para recuperar información del servidor.
* **Parámetro `id=wiener`:** Este es nuestro **vector de ataque**. Representa cómo la aplicación identifica qué perfil mostrar.

Podemos observar que el `id=wiener` está asociado a una cookie de sesión y, en la respuesta (**Response**), encontramos lo que buscamos: la **API KEY**.

<img width="1022" height="357" alt="Image" src="https://github.com/user-attachments/assets/d616fd18-cda7-4bdf-a9ca-d63e9a7fe7f4" />

---

### El Exploit: Escalada de Privilegios Horizontal (IDOR)
Como el objetivo no es la llave de `wiener`, sino la de `carlos`, movemos la petición al módulo **Repeater** para manipular los parámetros. Esto es lo que técnicamente conocemos como un **IDOR** (*Insecure Direct Object Reference*).

<img width="1001" height="567" alt="Image" src="https://github.com/user-attachments/assets/3f358e46-27a1-46be-b393-b69e3c190998" />

En el Request original tenemos el `id=wiener`. Para comprometer la cuenta de la víctima, simplemente cambiamos el valor del ID por `carlos`.

<img width="490" height="257" alt="Image" src="https://github.com/user-attachments/assets/05225299-64c8-4b84-b933-c476cd41553e" />

### Resultado Final
Al enviar la petición modificada, el servidor nos entrega la **API Key de Carlos** en el Response. Misión cumplida.

<img width="480" height="319" alt="image" src="https://github.com/user-attachments/assets/b3193741-8ee1-4c18-91cf-bb98ca413acd" />


