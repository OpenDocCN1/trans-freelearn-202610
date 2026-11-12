# # The spi-tools project

To quickly establish a connection with an SPI device from a shell script, for example, we suggest you use a small package that a French engineer (Christophe Blaess) has recently developed, which simplifies configuration and bidirectional communication. It is a free project called **spi-tools**.

This project is divided into two tools:

+   * **spi-config**: This allows you to view or edit the communications settings on a an SPI port. Here is the command:
+   
+   $ spi-config

+   * **spi-pipe**: This allows full-duplex talk with a device by redirecting standard input and standard output to the specified SPI port. The command is as follows:
+   
+   $ spi-pipe

# ## Note

If you want more information, consult the readme file in the GitHub repository: [https://github.com/cpb-/spi-tools](https://github.com/cpb-/spi-tools)