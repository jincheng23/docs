# Design Doc: Var_desc

## Background
PaddlePaddle divides the description of neural network computation into two stages: compile time and runtime. At compile time, the neural network computation is described as a `ProgramDesc` whereas at runtime an `Executor` interprets the `ProgramDesc` to compute the operations.

PaddlePaddle uses proto message to describe compile time program because :

1. The computation program description must be serializable and saved in a file.
1. During distributed training, the serialized program will be sent to multiple workers. It should also be possible to break the program into different components, each of which can be executed on a different worker.

The computation `Program` consists of nested `Blocks`. Each `Block` will consist of data(i.e. `Variable`)  and  `Operations`. The concept to represent them is in the table below.

<table>
<thead>
<tr>
<th></th>
<th>compile time</th>
<th>runtime</th>
</tr>
</thead>
<tbody>
<tr>
<td>Data </td>
<td>VarDesc(proto) </td>
<td>Variable(cpp) </td>
</tr>
<tr>
<td>Operation </td>
<td>OpDesc(proto) </td>
<td>Operator(cpp) </td>
</tr>
</tbody>
</table>


## Definition of VarType

A VarDesc should have a name, type and whether or not it is persistable. There are different kinds of variable types supported in PaddlePaddle, apart from the POD_Types like: `DENSE_TENSOR`, `SELECTED_ROWS`, `FEED_MINIBATCH`, `FETCH_LIST`, `STEP_SCOPES`, `LOD_RANK_TABLE`, `DENSE_TENSOR_ARRAY`, `PLACE_LIST`, `READER` and `CHANNEL`. These are declared inside `VarType`. A `VarDesc` then looks as the following:

```proto
message VarDesc {
  required string name = 1;
  required VarType type = 2;
  optional bool persistable = 3 [ default = false ];
}
```

## Definition of TensorDesc

```proto
message TensorDesc {
  // Should only be PODType. Is enforced in C++
  required Type data_type = 1;
  repeated int64 dims = 2; // [UNK, 640, 480] is saved as [-1, 640, 480]
}
```

The `Type` here comes from the enum defined inside of `VarType` :

```proto
enum Type {
  // Pod Types
  BOOL = 0;
  INT16 = 1;
  INT32 = 2;
  INT64 = 3;
  FP16 = 4;
  FP32 = 5;
  FP64 = 6;

  // Other types that may need additional descriptions
  DENSE_TENSOR = 7;
  SELECTED_ROWS = 8;
  FEED_MINIBATCH = 9;
  FETCH_LIST = 10;
  STEP_SCOPES = 11;
  LOD_RANK_TABLE = 12;
  DENSE_TENSOR_ARRAY = 13;
  PLACE_LIST = 14;
  READER = 15;
  CHANNEL = 16;
}
```

A TensorDesc describes `SelectedRows` and `DenseTensor`. For details of `SelectedRows`, please reference `SelectedRows` .

## Definition of LodTensorDesc

```proto
message DenseTensorDesc {
  required TensorDesc tensor = 1;
  optional int32 lod_level = 2 [ default = 0 ];
}
```

A DenseTensorDesc contains a tensor and a lod_level.

## Definition of Variable in Python

For Variable in Python, please reference `Python API`.
