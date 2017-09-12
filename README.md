# Ejemplo base:
Es lo que yo entendi, si alguien sabe mas puede editarlo

se crea el objeto simulador:
>set ns [new Simulator]

Seleccionar colores:
>$ns color 1 Green
>$ns color 2 Yellow
>$ns color 3 Red


archivo para almacenar resulatdos de salida:
>set nf [open out.nam w]

Para imprimir las graficas:
>$ns namtrace-all $nf

Procedimiento de finalizacion:
>proc finish {} {
        global ns nf
        $ns flush-trace
        close $nf
        exec nam out.nam &
        exit 0
}

Declara 4 estaciones y asigna variables(NODOS)
>set n0 [$ns node]  
>set n1 [$ns node]  
>set n2 [$ns node]  
>set n3 [$ns node]  
>set n4 [$ns node]  
>set n5 [$ns node]  

caracteristica del enlace que conectan las estaciones(NODOS)
>$ns duplex-link $n0 $n2 1Mb 11ms DropTail  
>$ns duplex-link $n1 $n2 1Mb 11ms DropTail  
>$ns duplex-link $n2 $n4 1Mb 5ms DropTail  
>$ns duplex-link $n3 $n4 10Mb 11ms DropTail  
>$ns duplex-link $n4 $n5 1Mb 14ms DropTail  

Orientacion de enlace
generar el fichero de animación .nam, y
describen la orientación de las estaciones y los enlaces.
>$ns duplex-link-op $n0 $n2 orient right-down  
>$ns duplex-link-op $n1 $n2 orient right-up  
>$ns duplex-link-op $n2 $n4 orient righ  
>$ns duplex-link-op $n3 $n4 orient right-up  
>$ns duplex-link-op $n4 $n5 orient righ  

>$ns duplex-link-op $n2 $n3 queuePos 0.5  

declaran un agente de transporte UDP en la estación n0,
y le asocian un tráfico CBR.


>set udp0 [new Agent/UDP]  
>$udp0 set class_ 1  
>$ns attach-agent $n0 $udp0  
>set cbr0 [new Application/Traffic/CBR]  
>$cbr0 set packetSize_ 500  
>$cbr0 set interval_ 0.005  
>$cbr0 attach-agent $udp0  


>set udp1 [new Agent/UDP]  
>$udp1 set class_ 2  
>$ns attach-agent $n1 $udp1  


>set cbr1 [new Application/Traffic/CBR]  
>$cbr1 set packetSize_ 500  
>$cbr1 set interval_ 0.005  
>$cbr1 attach-agent $udp1  


>set udp2 [new Agent/UDP]  
>$udp2 set class_ 3  
>$ns attach-agent $n3 $udp2  


>set cbr2 [new Application/Traffic/CBR]  
>$cbr2 set packetSize_ 500  
>$cbr2 set interval_ 0.005  
>$cbr2 attach-agent $udp2  

>set null0 [new Agent/Null]  
>$ns attach-agent $n5 $null0  
>$ns connect $udp0 $null0  
>$ns connect $udp1 $null0  
>$ns connect $udp2 $null0  



Comienzo y final para las aplicaciones cbr
>$ns at 0.0 "$cbr0 start"  
>$ns at 0.0 "$cbr1 start"  
>$ns at 0.0 "$cbr2 start"  
>$ns at 4.0 "$cbr1 stop"  
>$ns at 4.5 "$cbr0 stop"  
>$ns at 4.5 "$cbr2 stop"  

Procedimientio finish
Especifica que terminara en el segundo 5
>$ns at 5.0 "finish"  

Comenzar simulacion
>$ns run  



