# Runtime

- linux

```java
/**
 * 执行命令 exit结尾
 * @param cmds
 * @return
 */
public static String executeCmds(String[] cmds) {
   StringBuilder result = new StringBuilder();
   File wd = new File("/bin");
   Process proc = null;
   try {
      proc = Runtime.getRuntime().exec("/bin/bash", null, wd);
   }
   catch (IOException e) {
      log.error(e.getMessage());
      e.printStackTrace();
      return "IOExeception before cmd execute";
   }
   if (proc != null) {
      BufferedReader in = new BufferedReader(new InputStreamReader(proc.getInputStream()));
      PrintWriter out = new PrintWriter(new BufferedWriter(new OutputStreamWriter(proc.getOutputStream())), true);
      for(int i=0;i<cmds.length;i++)
         out.println(cmds[i]);
      try {
         String line;
         while ((line = in.readLine())!=null){
            result.append(line).append("\n");
         }
         proc.waitFor();
         in.close();
         out.close();
         proc.destroy();
      }
      catch (Exception e) {
         log.error(e.getMessage());
         e.printStackTrace();
         return "Exeception happens when cmd execute";
      }
   }
   return result.toString();
}
String result = executeCmds(new String[]{"ls -l","exit"});
```

```xml
        <dependency>
            <groupId>com.jcraft</groupId>
            <artifactId>jsch</artifactId>
            <version>0.1.54</version>
        </dependency>
```




```java
	/**
	 * ssh 执行 命令行
	 * @param host 主机名
	 * @param port 端口，默认22
	 * @param user 用户名
	 * @param password 密码
	 * @param command 命令行语句
	 * @return 结果
	 * @throws JSchException 远程异常
	 * @throws IOException io异常
	 */
	public static String sshCmd(String host, int port, String user, String password, String command) throws JSchException, IOException {
		JSch jsch = new JSch();
		Session session = jsch.getSession(user, host, port);
		session.setConfig("StrictHostKeyChecking", "no");
		//    java.util.Properties config = new java.util.Properties();
		//   config.put("StrictHostKeyChecking", "no");
		session.setPassword(password);
		session.connect();

		ChannelExec channelExec = (ChannelExec) session.openChannel("exec");
		InputStream in = channelExec.getInputStream();
		channelExec.setCommand(command);
		channelExec.setErrStream(System.err);
		channelExec.connect();
		String out = IOUtils.toString(in,"utf-8");
		channelExec.disconnect();
		session.disconnect();
		return out;
	}
```

