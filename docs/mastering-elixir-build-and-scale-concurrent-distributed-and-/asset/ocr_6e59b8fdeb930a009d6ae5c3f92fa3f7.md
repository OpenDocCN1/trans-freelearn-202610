By setting the `:vm_args` value, we're telling Distillery that we want to use our own `vm.args` file, living in `rel/custom.vm.args`.

A `vm.args` file lets us define which arguments are passed to the Erlang VM. If we don't specify any custom `vm.args`, this is the one Distillery would generate for us by default:

```
# Name of the node
-name elixir_drip@127.0.0.1

# Cookie for distributed erlang
-setcookie a_random_big_and_complex_cookie_value

# Enable SMP automatically based on availability
-smp auto
```

As you can see, by default, Distillery passes three parameters to the VM: the first one, `name`, sets the name of the Erlang node with the `<name>@<host>` format; the second one, `setcookie`, sets the cookie you need to use if you want to connect to the Erlang node; the third parameter, `smp`, controls whether the Erlang runtime should be started with the Symmetric Multiprocessing feature enabled or not.

![](bbox=[0.12, 0.48, 0.2, 0.58])

You can check all the available Erlang VM parameters in the Erlang documentation at http://erlang.org/doc/man/erl.html#init_flags. Here, we will only use the parameters that Distillery sets by default.

Because we are pointing to a custom `vm.args` file in the `:elixir_drip` release configuration, Distillery will, instead of generating a `vm.args` file from scratch, generate `vm.args` based on the `rel/custom.vm.args` file:

```
$ cat rel/custom.vm.args
-name <%= release_name %>@${POD_IP}

-setcookie ${ERLANG_COOKIE}

-smp auto
```

The previous file will be interpreted as an **Embedded Elixir (EEx)** template, so `<%= release_name %>` will become `elixir_drip`, the name of the release configuration. The other difference lies in the way we are setting the host and cookie values. Instead of the actual values, we're using `${POD_IP}` and `${ERLANG_COOKIE}` as placeholders for the real values.