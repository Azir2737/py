import ctypes
import time

# Estrutura POINT para receber as coordenadas x e y
class POINT(ctypes.Structure):
    _fields_ = [("x", ctypes.c_long), ("y", ctypes.c_long)]

# Função para obter a posição atual do cursor
def get_mouse_position():
    pt = POINT()
    ctypes.windll.user32.GetCursorPos(ctypes.byref(pt))
    return pt.x, pt.y

# Função principal para mostrar as coordenadas do mouse
def mostrar_posicao_mouse():
    try:
        while True:
            x, y = get_mouse_position()
            print(f"Posição atual do mouse: x={x}, y={y}", end="\r")
            time.sleep(0.1)
    except KeyboardInterrupt:
        print("\nExecução interrompida.")

# Executa a função
if __name__ == "__main__":
    mostrar_posicao_mouse()
