Tests are done with [PyTest](https://docs.pytest.org/en/latest/)

Basic full testing:
```shell
$ pytest  tests/
```

Useful pytest keys:

- `-s` - see output
- `-v` - verbose
- `-x` - stop after first failure

run specific test: `pytest  tests/test_comression_rate.py::test_kernel`

Options for tests:

- `--pool /path/to/` - path to pool directory (can be empty), saves bandwidth
- `--vmroot /path/to/` - path to debian virtual machine to test debian plugin

Kernel plugin testing uses (now) https://cdn.kernel.org/pub/linux/kernel/v5.x/linux-5.0.10.tar.xz , if you will index it before, tests will work faster:
`hashget --submit https://cdn.kernel.org/pub/linux/kernel/v5.x/linux-5.0.10.tar.xz --project kernel.org`

