## EXPLICACIÓN 

### EJERCICIO 1: UTILIZA DEBUG

- ***SCRIPT: CollisionDetector.cs***
- ***Se le asignó este script al cubo, es decir, al Enemy***

***VARIANTES EXPLICADAS***

- ***OnCollisionEnter: Esto se ejecuta cuando el enemigo entra en contacto con el jugador***
- ***OnCollisionStay: Esta variante se ejecuta cuando el enemigo sigue en contacto con el jugador, mientras dura el frame***
- ***OnCollisionExit: Se ejecuta cuando el enemigo se separa del jugador***

***Para que esto funcione hay que asignarle el tag player a la bola para identificar correctamente la colisión***

```C#

using UnityEngine;

public class CollisionDetector : MonoBehaviour
{
    // LLAMAMOS A LA COLISIÓN
    private void OnCollisionEnter(Collision collision)
    {
        if (collision.gameObject.CompareTag("Player"))
        {
            Debug.Log("¡COLISIIIIOÓNN! EL JUGADOR/BOLA TOCÓ AL ENEMIGO/CUBO.");
        }
    }

    // SE LLAMA A LA COLISIÓN MIENTRAS DURA EL FRAME
    private void OnCollisionStay(Collision collision)
    {
        if (collision.gameObject.CompareTag("Player"))
        {
            Debug.Log("ESTÁAAN COLISIONANDO... (LA BOLA SIGUE TOCANDO EL CUBOOO)");
        }
    }

    // SE LLAMA CUANDO LA COLISIÓN TERMINA
    private void OnCollisionExit(Collision collision)
    {
        if (collision.gameObject.CompareTag("Player"))
        {
            Debug.Log("SE ACABÓ LA COLISIÓOONN, LA BOLA SE SEPARÓ DEL CUBO.");
        }
    }
}

```

---

### EJERCICIO 2: PERSECUCIÓN CON ESTADOS LEJOS - CERCA

- ***SCRIPT: EnemyFollower.cs***
- ***Se le asignó este script al cubo, es decir, al Enemy***


 ***Se implementa una máquina de estados mediante un enum, tiene dos variantes; LEJOS y CERCA.***

- ***Estado LEJOS: Cuando la distancia del enemigo a la bola es mayor de la variable que declare como rango de detección, el cubo permanece estático***
  
- ***Estado CERCA: Cuando la distancia es menor o igual al valor de rango de detección, el cubo/enemigo calculará la dirección hacia la bola, moviendose hacia ella en cada frame***

```C#

using UnityEngine;

public class EnemyFollower : MonoBehaviour
{
    // REFERENCIAMOS AL JUGADOR / LA BOLA
    public Transform player;
    
    // DISTANCIA / RANGO A LA QUE EMPIEZA A PERSEGUIR
    public float detectionRange = 5f;
    
    // VELOCIDAD DE PERSECUCIÓN
    public float speed = 3f;

    // VARIABLE PARA GUARDAR EL ESTADO ACTUAL / SI ESTÁ LEJOS O CERCA DEL JUGADOR
    private string currentState = "LEJOS"; 

    void Update()
    {
        // SI NO SE ASIGNA EL JUGADOR NO SE HACE NADA
        if (player == null)
        {
            Debug.LogWarning("ASIGNA JUGADOR");
            return;
        }

        // CALCULAR DISTANCIA ENTRE CUBO Y BOLA
        float distance = Vector3.Distance(transform.position, player.position);

        // LÓGICA DE LOS ESTADOS LEJOS Y CERCA
        if (distance > detectionRange)
        {
            // LEJOS
            currentState = "LEJOS";
            // EL CUBO NO SE MUEVE
            Debug.Log("ESTADO: LEJOS - EL CUBO ESTÁ QUIETO NO SE MUEVEEEE");
        }
        else
        {
            // CERCA
            currentState = "CERCA";
            
            // DIRECCIÓNM HACIA EL JUGADOR
            Vector3 direction = (player.position - transform.position).normalized;
            
            // MOVIMIENTO DEL CUBO HACIA EL JUGADOR
            transform.position += direction * speed * Time.deltaTime;
            
            Debug.Log("ESTADO: CERCA - EL CUBO ESTA PERSIGUIENDO A LA BOLA");
        }
    }
}



```
