# 사이트 방문자의 IP 와 Country 가져오기

geoip 를 nuget 패키지 관리자에서 설치한 이후에, mmdb도 넣어주어야 한다.

패키지 관리자에서 검색시 maxmind 라고 검색해서
maxmind.db 와 maxmind.geoip2 두개를 설치해주어야 한다.

mmdb는 https://dev.maxmind.com/geoip/geoip2/geolite2/

여기서 다운로드 받는데, country 와 city 중에 필요에 맞게 설치해서 한다.

```
    public string GetVisitorsIP()
    {
        string VisitorsIP = string.Empty;
        if (System.Web.HttpContext.Current.Request.ServerVariables["HTTP_X_FORWARDED_FOR"] != null)
        {
            VisitorsIP = System.Web.HttpContext.Current.Request.ServerVariables["HTTP_X_FORWARDED_FOR"].ToString();
        }
        else if (System.Web.HttpContext.Current.Request.UserHostAddress.Length != 0)
        {
            VisitorsIP = System.Web.HttpContext.Current.Request.UserHostAddress;
        }

        return VisitorsIP;
    }

    public string GetCountryByIP(string ip)
    {
        if (ip == "::1" || ip == "127.0.0.1")
            return "Dev";

        using (var reader = new DatabaseReader(Server.MapPath("~/App_Data/GeoLite2-Country.mmdb")))
        {
            var geoIp = reader.Country(ip);
            return geoIp.Country.Name;
        }
    }
```