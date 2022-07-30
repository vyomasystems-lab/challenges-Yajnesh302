# RAM Design Verification

The verification environment is setup using [Vyoma's UpTickPro](https://vyomasystems.com) provided for the hackathon.

## Verification Environment

The [CoCoTb](https://www.cocotb.org/) based Python test is developed as explained.In the write data section of code in place of data_write, data_read is the bug in code

The following error is seen:
```
 File "/workspace/challenges-Yajnesh302/level3_design/ram/test_ram.py", line 52, in test_ram
                         value = yield read_ram(dut, i)
                       File "/workspace/challenges-Yajnesh302/level3_design/ram/test_ram.py", line 28, in read_ram
                         raise ReturnValue(int(dut.data_read.value))  # Read back the value
                       File "/workspace/.pyenv_mirror/fakeroot/versions/3.8.13/lib/python3.8/site-packages/cocotb/binary.py", line 509, in __int__
                         return self.integer
                       File "/workspace/.pyenv_mirror/fakeroot/versions/3.8.13/lib/python3.8/site-packages/cocotb/binary.py", line 336, in integer
                         return self._convert_from(self._str)
                       File "/workspace/.pyenv_mirror/fakeroot/versions/3.8.13/lib/python3.8/site-packages/cocotb/binary.py", line 231, in _convert_from_unsigned
                         return int(x.translate(_resolve_table), 2)
                       File "/workspace/.pyenv_mirror/fakeroot/versions/3.8.13/lib/python3.8/site-packages/cocotb/binary.py", line 77, in __missing__
                         return self.resolve_x(key)
                       File "/workspace/.pyenv_mirror/fakeroot/versions/3.8.13/lib/python3.8/site-packages/cocotb/binary.py", line 59, in resolve_error
                         raise ValueError("Unresolvable bit in binary string: '{}'".format(chr(key)))
                     ValueError: Unresolvable bit in binary string: 'x'
```
## Test Scenario
- Adress data should be written on the memory location from the variable data_write.
- Expected condition: The data should be written to the memory from the variable data_write and the design should work properly. 
-Error occured: But in the buggy design the variable is assigned as data_read, because of this  wrong variable the data is not written in the memory.

## Design Bug
Based on the above test input and analysing the design, we see the following

```
// Write data to memory
  always @(posedge clk_write) begin
    if (write_enable) begin
      memory[address_write] <= data_read;       ===> BUG
    end
  end
```
According to the design it should be data_write instead of data_read.

## Design Fix
Updating the design and re-running the test makes the test pass.