package com.p;
import java.sql.*;
public class DAO {
 Connection conn = null;
 PreparedStatement ps = null;
 ResultSet rs = null;
 public boolean registerTicket(Ticket t)
 {
	 boolean b = false;
	 try{
		 conn = DBUtil.getConnection();
		 ps = conn.prepareStatement("insert into TBL_tickets_1355596 values (?,?,?,?,?)");
		 ps.setInt(1, t.getTicketNo());
		 ps.setString(2,t.getTicketType());
		 ps.setString(3, t.getStatus());
		 ps.setString(4, t.getRaisedBy());
		 ps.setString(5,t.getAssignedTo());
		 int t1 = ps.executeUpdate();
		 if(t1>0)
		 {
			 b = true;
		 }
	 }
	 catch(SQLException e)
	 {
		 e.printStackTrace();
	 }
	 finally
	 {
		 DBUtil.closeAllConnection(conn, ps, null);
	 }
	 
	 
	 return b;
	 
 }
 public boolean validateTicket(int t)
 {
	 boolean validated = false;
	 boolean present = false;
	 try{
		 conn = DBUtil.getConnection();
		 ps = conn.prepareStatement("select ticketno from TBL_tickets_1355596 ");
		 rs = ps.executeQuery();
		 while(rs.next())
		 {
			 if (rs.getInt(1)== t)
			 {
				 present = true;
				 break;
			 }
		 }
		 if(!present)
			 validated = true;
	 }
	 catch(SQLException e)
	 {
		 e.printStackTrace();
	 }
	 finally
	 {
		 DBUtil.closeAllConnection(conn, ps, null);
	 }
	 return validated;
 }
 public Ticket getTicket(int id)
 {
	 Ticket t = new Ticket();
	 try{
		 conn = DBUtil.getConnection();
		 ps = conn.prepareStatement("select * from TBL_tickets_1355596 where ticketno = ?");
		 ps.setInt(1, id);
		 rs = ps .executeQuery();
		 while (rs.next())
		 {
			 t.setTicketNo(id);
			 t.setTicketType(rs.getString(2));
			 t.setStatus(rs.getString(3));
			 t.setRaisedBy(rs.getString(4));
			 t.setAssignedTo(rs.getString(5));
		 }
	 }
	 catch(SQLException e)
	 {
		 e.printStackTrace();
	 }
	 finally
	 {
		 DBUtil.closeAllConnection(conn, ps, rs);
	 }
	 return t;
 }
}
