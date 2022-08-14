# Getting started

## Installation

- download <https://github.com/mfr-driverless/driverless-framework/releases/tag/v0.2.3>
- execute `pip install <path to zip>/driverless_framework-0.2.3.tar.gz`

> **_NOTE:_**  The framework must be installed on the windows computer as well as in the WSL.

---

## Project creation

The following steps should be created

### Data Enum

```python
from driverless_framework.architecture.basic_objects import IDataID
import enum

class DataID(IDataID):
    TEST_DATA = 1 
    TEST_DATA2 = 1 
```

### Nodes

#### Data sources

```python
from <path to id enum>.DataID import DataID
from driverless_framework.architecture.basic_objects import IDataSource
from driverless_framework.architecture.data_structure import DataGroup
from driverless_framework.architecture.data_structure import DataField

class Source(IDataSource):

    def __init__(self):
        super().__init__()
        self.__registeredMethods = []

    def getName(self):
        return "Source"

    def registerMethod(self, dataID):
        print("Register: "+str(dataID))
        self.__registeredMethods.append(dataID)

    def start(self):
        while True:
            if DataID.TEST_DATA in self.__registeredMethods:
                test_data = ... # Get required data
                self._insertData(DataID.TEST_DATA, DataField(test_data))

            if len(self.__registeredMethods) > 0:
                print("Data collected...")
                self._callUpdate()
```

> **_ℹ INFO:_**  This is a standard implementation that should be adapted to the use case. It is important that the methods are present!

- `registerMethod()` wird vor start des Managers aufgerufen. Hier werden der Source die angefragten IDs übergeben, welche wenn möglich bereitgestellt werden sollten
- `start()` is called once at startup. With `_insertData()` datasets can be provided. They are pushed into the pipeline by executing `_callUpdate()`.
- In `_insertData()` records must be encapsulated in a `DataField` object. The data length of the entire data can also be passed here. However, this is usually only used for recorded data, since otherwise this information is not yet known at runtime.

&nbsp;

#### Data processors

```python
from driverless_framework.architecture.node_types import DataProcessor
from driverless_framework.architecture.decorators.node_decorator import node
from <path to id enum>.DataID import DataID


class Processor(DataProcessor):

    def getName(self):
        return "Processor"

    @node([DataID.TEST_DATA, DataID.TEST_DATA2]) # requests datasets
    def receive(self, data):
        test_data = data.getData(DataID.TEST_DATA).data # gets requested data
        test_data2 = data.getData(DataID.TEST_DATA2).data

        self._publish(DataID.TEST_DATA3, ...) # pipes calculated data to next nodes
```

> **_ℹ INFO:_**  it is recommended not to use an already existing id in the publish method, otherwise the following nodes can no longer access the original dataset!

&nbsp;

#### End nodes

```python
from driverless_framework.architecture.node_types import EndNode
from driverless_framework.architecture.decorators.node_decorator import node
from <path to id enum>.DataID import DataID

class ENode(EndNode):

    def getName(self):
        return "ENode"

    @node([DataID.TEST_DATA]) # requests datasets
    def receive(self, data):
        test_data = data.getData(DataID.TEST_DATA).data # gets requested data
```

> **_ℹ INFO:_**  an endNode is comparable to a dataprocessor, except that you can't publish (pass on) data.

&nbsp;

### Manager

```python
import driverless_framework.architecture as a

# Source(), EndNode() and Processor() has to be replaced
manager = a.ManagerBuilder(Source(), EndNode()) \
    .addProcessorNode(Processor()) \
    .build()

manager.start()
```

> **_ℹ INFO:_**  The manager merges the notes into a pipeline and executes it. The further behavior is primarily regulated in the source node.

&nbsp;

---

## Special functions

### Data recording

### Global variables

---

## Testing
