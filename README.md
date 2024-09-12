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

# Função para obter o tamanho da tela principal
def get_screen_size():
    user32 = ctypes.windll.user32
    user32.SetProcessDPIAware()  # Para capturar o tamanho correto em sistemas com DPI diferente
    screen_width = user32.GetSystemMetrics(0)
    screen_height = user32.GetSystemMetrics(1)
    return screen_width, screen_height

# Função principal para mostrar as coordenadas do mouse e tamanho da tela
def mostrar_posicao_mouse():
    screen_width, screen_height = get_screen_size()
    print(f"Tamanho da tela: largura={screen_width}, altura={screen_height}")
    
    try:
        last_x, last_y = None, None  # Variáveis para armazenar a última posição
        while True:
            x, y = get_mouse_position()
            
            # Exibe a posição do mouse e verifica se está dentro da tela
            if (x != last_x or y != last_y):
                print(f"Posição atual do mouse: x={x}, y={y} (dentro da tela? {0 <= x <= screen_width and 0 <= y <= screen_height})", end="\r")
                last_x, last_y = x, y  # Atualiza a última posição

            time.sleep(0.5)
    except KeyboardInterrupt:
        print("\nExecução interrompida.")

# Executa a função
if __name__ == "__main__":
    mostrar_posicao_mouse()

