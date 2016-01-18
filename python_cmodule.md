Following along with this video set  

1. https://www.youtube.com/watch?v=s6cvSkbWG3s  
2. https://www.youtube.com/watch?v=bfmslcTKriw  

totally confused about this

```text
$  python setup.py build
$  sudo python setup.py install

```

exmodmodule.c

```C
#include <Python.h>

/*
int PyArg_ParseTuple(PyObject *args, const char *format, ...)
int PyArg_parseTupleAndKeywords(
    PyObject *args, 
    PyObject *kw, 
    const char *format,
    char *keywords[],..)
int PyArg_Parse(PyObject *args, const char *format, ....)

format represents a Format Unit, a way to extra an expected type object data
from Python. Usually a single letter, but is allowed more

   s (constant char*)
   b (unsigned char)
   h (short in)
   i (int)
   l (long)
   d (double)
   O (PyObject)

these funcs return 1 on success and 0 on error. If there is an error 
you want to raise an exception

PyObject * Py_BuildValue(const char *format, ...)


*/


//Define a new exception object for our module
static PyObject* ExmodError;

static PyObject* exmod_say_hello(PyObject* self, PyObject* args){
  const char* msg;
  int sts = 0;
  
  // we except at least 1 string argument to this function
  if (!PyArg_ParseTuple(args, "s", &msg)){
  	return NULL;  
  }

  // check if there is any problem with the information passes
  // from python to this module
  if (strcmp(msg, "this_is_an_error") == 0){
  	// if error, raise an exception with out exception object
  	PyErr_SetString(ExmodError, "EXCPT ERR after typing an expected invalid response");
  	return NULL; // propagate the error to called (Python interpreter)
  }
  else{
  	//no error found, continue
  	printf("This is C world\nyour message is: %s\n", msg);
  	sts = 21;
  }
  return Py_BuildValue("i", sts);

}

static PyObject* exmod_add(PyObject* self, PyObject* args){
    double a,b;
    double sts=0;
    // we expect at laeast 1 string arugment to this function
    if (!PyArg_ParseTuple(args, "dd", &a, &b)){
    	return NULL; // return error if none found
    }
    sts = a + b;
    printf("This is C world\n Addition of %f + %f = %f", a, b, sts);
    return Py_BuildValue("d", sts);
}

static PyMethodDef exmod_methods[] = {
	//"PythonNAme",  C-function Name,    argument_presentation,    description
	{"say_hello",    exmod_say_hello,    METH_VARARGS,             "Say Hello from C and print MSG"},
	{"add",          exmod_add,          METH_VARARGS,             "Add two number in C"},
	{NULL, NULL, 0, NULL}   /*sentinal*/
};

PyMODINIT_FUNC_initexmod(void){
    PyObject* m;
    m = Py_InitModule("exmod", exmod_methods);
    if (m == NULL) return;

   	ExmodError = PyErr_NewException("exmod.error", NULL, NULL); // exmod error python error object
    Py_INCREF(ExmodError);

    PyModule_AddObject(m, "error", ExmodError);


}
```
setup.py

```python
from distutils.core import setup, Extension

module1 = Extension(
    'exmod',
    include_dirs=['/usr/local/include'],
    libraries=['pthread'],
    sources=['exmodmodule.c']
)

setup(
    name='exmod',
    version='1.0',
    description='this is an example package',
    author='argargarg',
    url='http://www.moooooo.com',
    ext_modules=[module1]
)

```

I get this error on import
```text
>>> import exmod
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ImportError: dynamic module does not define init function (initexmod)
```


