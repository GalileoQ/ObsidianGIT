#### Ejecución del reenvío de puerto local por ssh
```python
ssh -L 1234:localhost:3306 ubuntu@10.129.202.64
```

#### Reenvío de múltiples puertos

Reenvío dinámico de puertos con túnel SSH y SOCKS

```python
ssh -L 1234:localhost:3306 -L 8080:localhost:80 ubuntu@10.129.202.64
```

