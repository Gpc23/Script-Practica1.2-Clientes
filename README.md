# Script-Practica1.2-Clientes

#!/bin/bash

NOMBRE_MAQUINA="$1"
TAMANO_VOLUMEN="$2"
NOMBRE_RED="$3"

PLANTILLA="/home/gonzalopc/srv/plantilla-cliente.qcow2"
NUEVO_VOLUMEN="/home/gonzalopc/srv/discos/$NOMBRE_MAQUINA.qcow2"

virt-clone --connect=qemu:///system --original plantilla-cliente --name "$NOMBRE_MAQUINA" --auto-clone --file "$NUEVO_VOLUMEN"

virt-customize --connect=qemu:///system -d "$NOMBRE_MAQUINA" --hostname "$NOMBRE_MAQUINA"

qemu-img resize "$NUEVO_VOLUMEN" "$TAMANO_VOLUMEN"

virsh start "$NOMBRE_MAQUINA"

virsh attach-interface --domain "$NOMBRE_MAQUINA" --type network --source "$NOMBRE_RED" --model virtio --config --live

echo "La máquina virtual $NOMBRE_MAQUINA ha sido creada."
