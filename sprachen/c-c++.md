# C/C++





## Pointer

```clike
#include <stdio.h>

int main(int argc, char** argv){
  
  // Pointer auf Speicherstelle mit Größe von genau einem Integerwert
  int *var = malloc(sizeof(int));
  if (var == NULL) {
    fprintf(stderr, "Speicherallokation fehlgeschlagen\n");
    return 1;
  }
  *var = 12;
  
  // Wert des Pointers, also die Adresse der Speicherstelle
  printf("Adresse der Speicherstelle: %p\n", (void*)var);
    
  // An dieser Speicherstelle gespeicherte Wert, also 12
  printf("Gespeicherter Wert: %d\n", *var);
    
  // Adresse des Pointers
  printf("Adresse des Pointers: %p\n", (void*)&var);
  
  // Speicher freigeben
  free(var);
  
  return 0;
}

```



