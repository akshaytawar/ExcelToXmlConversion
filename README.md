# ExcelToXmlConversion
This code will convert A Excel File to XML File In Java

import java.io.File;
import java.io.FileWriter;
import java.io.IOException;
import org.apache.poi.openxml4j.exceptions.InvalidFormatException;
import org.apache.poi.ss.usermodel.DataFormatter;
import org.apache.poi.ss.usermodel.Row;
import org.apache.poi.ss.usermodel.Sheet;
import org.apache.poi.ss.usermodel.Workbook;
import org.apache.poi.xssf.usermodel.XSSFWorkbook;


public class ExcelToXmlConversion {

	public static void convertToXml(File excelfile) {
		
				File xmlfile=new File(excelfile.toString().replaceAll(".xlsx", ".xml"));
				Workbook workbook;
				Sheet sheet;
				FileWriter fw=null;
				DataFormatter dataformatter=new DataFormatter();
				try {
					
          //To create XML File in same folders.
					if(!xmlfile.exists()){
						xmlfile.createNewFile();
					}
					
          //Create Workbook Object to represent Excel file.
					workbook = new XSSFWorkbook(excelfile);
					sheet=workbook.getSheetAt(0);
				  
          fw=new FileWriter(xmlfile);
					
          //Obtain first row(Header Row)
				  Row firstrow=sheet.getRow(0);
					
          //Name of root element
          fw.write("<name of root element>\n");
				    for(int i=1;i<=sheet.getLastRowNum();i++){
				    	  
				    	Row remrows=sheet.getRow(i);
				    	
              //Name of Element
              fw.write("\t<Name of Element>\n");
				    	for(int j=0;(j<firstrow.getLastCellNum() && j<remrows.getLastCellNum());j++){
				    		
	      fw.write("\t\t<"+firstrow.getCell(j)+">"+dataformatter.formatCellValue(remrows.getCell(j))+"</"+firstrow.getCell(j)+">\n");			
				    	}
				    	  fw.write("\t</Name of Element>\n");
				    }
			      
            fw.write("</name of root element>\n");
        
				   
				} catch (InvalidFormatException | IOException e) {
					// TODO Auto-generated catch block
					e.printStackTrace();
				}finally{
					
					if(fw!=null){
						try {
							fw.close();
						} catch (IOException e) {
							e.printStackTrace();
						}
					}
				}
				
			}

	
	public static void main(String [] args) {
		
	   File xmlfile=new File("Path of Excel File");	
	   ExcelToXmlConversion.convertToXml(xmlfile);	
		
	}
