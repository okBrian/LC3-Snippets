---
description: Here is an example of a ArrayList/Vector Implemented in C
---

# ArrayList/Vector Example

<pre class="language-c"><code class="lang-c">#include &#x3C;stdio.h>  
#include &#x3C;stdlib.h>


struct IntVector
{
  int* backingArray;
  int capacity;
  int size;
};

<strong>
</strong><strong>void init(struct IntVector* vector) {
</strong>  const int INITAL_CAPACITY = 10;
  vector->backingArray = (int*)(calloc(INITAL_CAPACITY, sizeof(int)));
  if (!vector->backingArray) {
    fprintf(stderr, "Init Calloc for Vector Failed");
    exit(1);
  }
  vector->capacity = INITAL_CAPACITY;
  vector->size = 0;
}

void addAtIndex(struct IntVector* vector, int index, int data) {
  if (index &#x3C; 0) {
    fprintf(stderr, "Index Out of Bounds Exception, Trying to insert at a index less than 0\n");
    exit(1);
  }
  if (index > vector->size) {
    fprintf(stderr, "Index Out of Bounds Exception, Trying to insert at a index greater than size\n");
    exit(1);
  }

  if (vector->size == vector->capacity) {
    int* temp = (int*)(realloc(vector->backingArray, vector->capacity * sizeof(int) * 2));
    if (!temp) {
      fprintf(stderr, "Resize Realloc for Vector Failed");
      exit(1);
    }
    vector->backingArray = temp;
    vector->capacity *= 2;
  }
  
  for (int i = vector->size - 1; i >= index; i--) {
    vector->backingArray[i + 1] = vector->backingArray[i];
  }

  vector->backingArray[index] = data;
  vector->size++;
}

int removeAtIndex(struct IntVector* vector, int index) {
  if (index &#x3C; 0) {
    fprintf(stderr, "Index Out of Bounds Exception, Trying to remove at a index less than 0\n");
    exit(1);
  }
  if (index >= vector->size) {
    fprintf(stderr, "Index Out of Bounds Exception, Trying to remove at a index greater than size\n");
    exit(1);
  }

  int out = *(vector->backingArray + index);

  if (vector->size == 1) {
    vector->backingArray[index] = 0;
    vector->size -= 1;
  } else {
    for (int i = index + 1; i &#x3C; vector->capacity; i++) {
      vector->backingArray[i - 1] = vector->backingArray[i];
    }
    vector->backingArray[vector->size - 1] = 0;
    vector->size -= 1;
  }

}

int get(struct IntVector* vector, int index) {
  if (index &#x3C; 0) {
    fprintf(stderr, "Index Out of Bounds Exception, Trying to access a index less than 0\n");
    exit(1);
  }
  if (index > vector->size) {
    fprintf(stderr, "Index Out of Bounds Exception, Trying to access a index larger than size\n");
    exit(1);
  }

  return *(vector->backingArray + index);
};

int isEmpty(struct IntVector* vector) {
  return vector->size == 0;
}

void clear(struct IntVector* vector) {
  free(vector->backingArray);
  vector->backingArray = (int*)(calloc(vector->capacity, sizeof(int)));
  if (!vector->backingArray) {
    fprintf(stderr, "Init Calloc for Vector Failed");
    exit(1);
  }
  vector->size = 0;
}

void shutdown(struct IntVector* vector) {
  free(vector->backingArray);
  free(vector);
}
</code></pre>
