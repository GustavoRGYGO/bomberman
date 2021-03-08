num_w=int(input())
cargas=input().split()
lista_w=[]
sobras=[]

i=0
while i<len(cargas):
    cargas[i]=int(cargas[i])
    i+=1

if num_w==0:
    print('Os Wookies foram para o lado sombrio da força!')
    i=0
    while i<len(cargas):
        print(f'{cargas[i]}',  end=' ')
        i+=1
        
elif num_w>0:
    i=0
    while i<num_w:
        lista_w.append([])
        i+=1
    i=0
    while i<num_w:
        if len(cargas)!=0:
            lista_w[i].append(cargas.pop(0))
        i+=1
    while not len(cargas)==0:
        i=0
        while i<num_w:
            if lista_w[i][-1]>=cargas[0]:
                lista_w[i].append(cargas.pop(0))
                break
            else:
                i+=1
        if i==num_w:
            sobras.append(cargas.pop(0))
        
    lista_w.sort(reverse=True, key=sum)
    
    i=0
    while i<num_w:
        print(f'{lista_w[i]}', end=' ')
        i+=1
    print()
    if len(sobras)==0:
        print('A força está com os Wookies!')
    else:
        i=0
        while i<len(sobras):
            print(f'{sobras[i]}',  end=' ')
            i+=1
    
