# Installation CASTEP - AOCL
## CASTEP 25.12 + AOCL (Ubuntu 24.04, Ryzen 3900X) — Guia Final

### 0) Pré-requisitos
```
sudo apt update
sudo apt install -y build-essential gfortran cmake git \
    libopenmpi-dev openmpi-bin \
    libfftw3-dev python3 python3-pip gawk
```
Confirme MPI
```
which mpif90
which mpirun
# Esperado:
# /usr/bin/mpif90
# /usr/bin/mpirun
```

### 1) Definir AOCL e diretórios de biblioteca

Você instalou o AOCL em:
/opt/AMD/aocl/aocl-linux-gcc-5.1.0

Defina variáveis e caminho das libs (LP64):

```
export AOCL=/opt/AMD/aocl/aocl-linux-gcc-5.1.0
export LIBDIR=$AOCL/gcc/lib_LP64
export LD_LIBRARY_PATH=$LIBDIR:$LD_LIBRARY_PATH

```

### 2) Ajustar symlinks das libs AOCL (uma vez só)

O CASTEP procura libblis.so e libflame.so. No AOCL encontramos:


```
/opt/AMD/aocl/aocl-linux-gcc-5.1.0/gcc/lib_LP64/libblis.so.5.1.0
/opt/AMD/aocl/aocl-linux-gcc-5.1.0/gcc/lib_LP64/libflame.so
```
Crie os symlinks (no mesmo diretório):
```
cd /opt/AMD/aocl/aocl-linux-gcc-5.1.0/gcc/lib_LP64
sudo ln -s libblis.so.5.1.0 libblis.so
sudo ln -s libblis-mt.so.5.1.0 libblis-mt.so
# libflame.so já tem o nome esperado
```

### 3) Descompactar o CASTEP
```
tar -zxf CASTEP-25.12.tar.gz
cd CASTEP-25.12
```


### 4) Compilar (serial e MPI) com AOCL + FFTW3
4.1 Serial (opcional, útil para utilitários)

```
make clean
make -j$(nproc) COMMS_ARCH=serial \
     MATHLIBS=aocl MATHLIBDIR="$LIBDIR" \
     FFT=fftw3  FFTLIBDIR="$LIBDIR" \
     BUILD=fast TARGETCPU=host
make COMMS_ARCH=serial install
```
Instala em:
```
bin/linux_x86_64_gfortran10--serial/
```
4.2 MPI (principal)
```
make clean
make -j$(nproc) COMMS_ARCH=mpi \
     MATHLIBS=aocl MATHLIBDIR="$LIBDIR" \
     FFT=fftw3  FFTLIBDIR="$LIBDIR" \
     BUILD=fast TARGETCPU=host
make COMMS_ARCH=mpi install

```

Instala em:
```
bin/linux_x86_64_gfortran10--mpi/

```
### 5) Verificar linkedição com AOCL

```
ldd obj/linux_x86_64_gfortran*/castep.mpi | grep -E "blis|flame"
# Esperado (caminhos AOCL):
# libflame.so => /opt/AMD/aocl/aocl-linux-gcc-5.1.0/gcc/lib_LP64/libflame.so
# libblis.so.5 => /opt/AMD/aocl/aocl-linux-gcc-5.1.0/gcc/lib_LP64/libblis.so.5
# libblis-mt.so.5 => /opt/AMD/aocl/aocl-linux-gcc-5.1.0/gcc/lib_LP64/libblis-mt.so.5

```

### 6) Teste rápido (MPI)

Com arquivos si.cell/si.param prontos, rode:
```
mpirun -np 4 ~/CASTEP/CASTEP-25.12/bin/linux_x86_64_gfortran10--mpi/castep.mpi si
```

(Ajuste -np conforme desejar.)

### 7) Persistir ambiente no ~/.bashrc

Abra o arquivo:

```
nano ~/.bashrc
```
Adicione ao final (usando os mesmos nomes de variáveis/comandos que usamos acima):
```
# === AOCL libraries ===
export AOCL=/opt/AMD/aocl/aocl-linux-gcc-5.1.0
export LD_LIBRARY_PATH=$AOCL/gcc/lib_LP64:$LD_LIBRARY_PATH

# === CASTEP binaries (MPI) ===
export PATH=$HOME/CASTEP/CASTEP-25.12/bin/linux_x86_64_gfortran10--mpi:$PATH
```
Salve e recarregue:
```
source ~/.bashrc
```

### 8) Wrapper para simplificar mpirun -np ... castep.mpi

Crie /usr/local/bin/castep-mpi:

```
sudo bash -lc 'cat > /usr/local/bin/castep-mpi << "EOF"
#!/bin/bash
# Uso: castep-mpi <nprocs> <seed> [args...]
NPROCS="$1"
SEED="$2"
shift 2
mpirun -np "$NPROCS" $HOME/CASTEP/CASTEP-25.12/bin/linux_x86_64_gfortran10--mpi/castep.mpi "$SEED" "$@"
EOF
chmod +x /usr/local/bin/castep-mpi'
```

Agora você pode rodar assim:
```
castep-mpi 4 si
```

### 9) Dúvidas comuns / Troubleshooting

MPI_ABORT ... errorcode 1 logo no início
Quase sempre é falta de pseudopotencial. Use OTFG no .cell:
```
%BLOCK SPECIES_POT
Si  OTFG  [Si] 3|2.0|2.0|2.0:2|6.0|8.0|10.0
%ENDBLOCK SPECIES_POT
```
ou defina PSPOT_DIR para .usp.

cannot find -lflame / -lblis
Garanta:

```
export LIBDIR=$AOCL/gcc/lib_LP64

export LD_LIBRARY_PATH=$LIBDIR:$LD_LIBRARY_PATH
```

symlinks criados (passo 2).

Utilitários reclamando diretório --serial ausente no make install
É normal se você instalou só MPI. Para instalar utilitários, também rode o build/instalação serial:

```
make COMMS_ARCH=serial BUILD=fast TARGETCPU=host
make COMMS_ARCH=serial install
```
Quantos processos usar?
Sua CPU tem 12 cores físicos / 24 threads. Comece testando:

castep-mpi 12 si (1 MPI por core físico)

compare com castep-mpi 24 si (1 MPI por thread)

e com castep-mpi 4 si (teste leve)

### 10) Exemplo final de execução
```
# com wrapper e 12 processos (recomendação inicial)
castep-mpi 12 si
```