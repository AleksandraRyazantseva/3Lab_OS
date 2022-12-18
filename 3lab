import threading
from collections import deque
import random
from time import sleep

def work(done, name, interval = 3):
    global dq, b, n
    while (not done.wait(interval)) and (len(dq) < n):
        with lock:
            if (len(dq) < n) and (b):
                dq.append(random.randint(1, 100))
                if len(dq) == n-1:
                    b = False
                print('Производитель', name,'добавил', dq[-1],', len =', len(dq))

def pop(done, name, interval = 3):
    global dq, b, l, n, m
    while (not done.wait(interval)) and (len(dq) > 0):
        with lock:
            if len(dq) > 0:
                if len(dq) >= n-1:
                    l = True
                if (l) and (len(dq) <= m):
                    l = False
                    b = True
                print('Потребитель', name, 'забрал', dq[0])
                dq.popleft()
    if done:   
        while len(dq) >= 1:
            with lock:
                if len(dq) >= 1:
                    print('Потребитель', name, 'забрал', dq[0])
                    dq.popleft()
            sleep(3)
            
q = ''
b = True
l = False
n = 100
m = 80
dq = deque([], 200)
dq.clear()
lock = threading.Lock()
done1 = threading.Event()

mark1 = threading.Thread(target = work, args = [done1, 1], daemon = True)
mark2 = threading.Thread(target = work, args = [done1, 2], daemon = True)
mark3 = threading.Thread(target = work, args = [done1, 3], daemon = True)

user1 = threading.Thread(target=pop, args=[done1, 1], daemon=True)
user2 = threading.Thread(target=pop, args=[done1, 2], daemon=True)

mark1.start()
mark2.start()
mark3.start()

user1.start()
user2.start()


while q != 'q':
    q = input('Нажмите "q" чтобы прекратить работу ')

done1.set()
