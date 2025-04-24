# Consultas SQL para Base de Datos Bancaria

## Enunciado 1: Clientes con múltiples cuentas y sus saldos totales

**Necesidad:** El banco necesita identificar a los clientes que tienen más de una cuenta, mostrar cuántas cuentas tiene cada uno y el saldo total acumulado en todas sus cuentas, ordenados por saldo total de mayor a menor.

**Consulta SQL:**
```sql
select c1.id_cliente, c1.nombre, sum(c2.saldo) AS SaldoTotal from cliente c1 join cuenta c2 on c1.id_cliente = c2.id_cliente  group by c1.id_cliente  order by SaldoTotal desc;

```

## Enunciado 2: Comparativa entre depósitos y retiros por cliente

**Necesidad:** El departamento de análisis financiero necesita comparar los montos totales de depósitos y retiros realizados por cada cliente, para identificar patrones de comportamiento financiero.

**Consulta SQL:**
```sql
select c1.num_cuenta, c1.id_cliente, sum(tr.monto) as monto, tr.tipo_transaccion from transaccion tr join cuenta c1 on tr.num_cuenta = c1.num_cuenta where tr.tipo_transaccion != 'tr.transferencia' group by c1.num_cuenta, c1.id_cliente, tipo_transaccion;

```

## Enunciado 3: Cuentas sin tarjetas asociadas

**Necesidad:** El departamento de tarjetas necesita identificar todas las cuentas que no tienen tarjetas asociadas para ofrecer productos específicos a estos clientes.

**Consulta SQL:**
```sql
select * from cuenta C1 left JOIN tarjeta ta 
on c1.num_cuenta = ta.num_cuenta
where ta.numero_tarjeta = null;
```

## Enunciado 4: Análisis de saldos promedio por tipo de cuenta y comportamiento transaccional

**Necesidad:** La gerencia necesita un análisis comparativo del saldo promedio entre cuentas de ahorro y corriente, pero solo considerando aquellas cuentas que han tenido al menos una transacción en los últimos 30 días.

**Consulta SQL:**
```sql
select c1.num_cuenta, avg(c1.saldo), c1.tipo_cuenta, tr.fecha as fecha_transaccion from cuenta c1 join transaccion tr on c1.num_cuenta = tr.num_cuenta
where tr.fecha >= CURRENT_DATE - INTERVAL '30 days'
group by c1.num_cuenta, tr.fecha
order by c1.saldo desc;
```

## Enunciado 5: Clientes con transferencias pero sin retiros en cajeros

**Necesidad:** El equipo de marketing digital necesita identificar a los clientes que utilizan transferencias pero no realizan retiros por cajeros automáticos, para dirigir campañas de banca digital.

**Consulta SQL:**
```sql
select * from cuenta c1 join transaccion tr on c1.num_cuenta=tr.num_cuenta where tipo_transaccion='transferencia' and descripcion != 'Retiro en cajero';
```
