var http=require('http');
var url=require('url');
var server=http.createServer(function(req,res){
	if(req.method !='GET') return res.end('Sólo peticiones GET');
	
	var ref=url.parse(req.url,true);
	var iso=ref.query['iso'];
	var result={};
	if (iso){
		res.writeHead(200,{'Content-Type':'application/json'})
		var date = new Date(iso);
		switch(ref.pathname)
		{
			case '/api/parsetime':
				result.hour=date.getHours();
				result.minute=date.getMinutes();
				result.second=date.getSeconds();
				break;
			case '/api/unixtime':
				result.unixtime=date.getTime();
				break;
		}
		return res.end(JSON.stringify(result));
	}
	else
	{
		return res.end('No existe el parámetro ISO');
	}
});
server.listen(Number(process.argv[2]));
