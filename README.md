# Telnet-Multiple-Ip

$TopIP = @("111.65.250.2","222.255.239.80")
$PortList = @(21,22)

$TopIP | ForEach-Object {
    
        $IP = Test-NetConnection $_ | Select ComputerName,TcpTestSucceeded,RemoteAddress,RemotePort
        $PortList | ForEach-Object{

        $TcpTestSucceeded = (Test-NetConnection $Domain.ComputerName -Port $_ -ErrorAction SilentlyContinue -WarningAction SilentlyContinue).TcpTestSucceeded

        $RemoteAddress = (Test-NetConnection $Domain.ComputerName -Port $_ -ErrorAction SilentlyContinue -WarningAction SilentlyContinue).RemoteAddress
        
        $RemotePort = (Test-NetConnection $Domain.ComputerName -Port $_ -ErrorAction SilentlyContinue -WarningAction SilentlyContinue).RemotePort
         
        $date = Get-Date

        if($TcpTestSucceeded -eq "True"){
             $TcpTestSucceeded = "Success"

        }else{
             $TcpTestSucceeded = "Failed"
             Write-Host "Telnet status  $TcpTestSucceeded  - To IP $RemoteAddress with port $RemotePort : $date" -ForegroundColor Red
        }   
    }
    Write-Host "Telnet status  $TcpTestSucceeded - To IP $RemoteAddress with port $RemotePort : $date" -ForegroundColor Green
} | FT

