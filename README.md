# plsql-ftp
PL/SQL FTP Package with explicit TLS support

## Examples
### Get File
    declare
      l_conn utl_tcp.connection;
      l_blob blob;
    begin
      l_conn := ftp.login (
                  p_host        => '192.168.0.1',
                  p_port        => '21',
                  p_user        => 'user',
                  p_pass        => 'pass',
                  p_timeout     => '15',
                  p_wallet_path => null,
                  p_wallet_pass => null
                );

      -- set binary tranfer
      ftp.binary(l_conn);

      l_blob := ftp.get_remote_binary_data (
                  p_conn => l_conn,
                  p_file => 'file.txt'
                );

      -- disconnect
      ftp.logout(l_conn);
    exception when others then
      begin
        ftp.logout(l_conn); -- force disconnect on error
      exception when others then null;
      end;

      raise;
    end;
    /

### Put File
    declare
      l_conn utl_tcp.connection;
      l_blob blob;
    begin
      l_conn := ftp.login (
                  p_host        => '192.168.0.1',
                  p_port        => '21',
                  p_user        => 'user',
                  p_pass        => 'pass',
                  p_timeout     => '15',
                  p_wallet_path => null,
                  p_wallet_pass => null
                );

      -- set binary tranfer
      ftp.binary(l_conn);

      ftp.put_remote_binary_data (
        p_conn => l_conn,
        p_file => 'file.txt'
        p_data => l_blob
      );

      -- disconnect
      ftp.logout(l_conn);
    exception when others then
      begin
        ftp.logout(l_conn); -- force disconnect on error
      exception when others then null;
      end;

      raise;
    end;
    /
