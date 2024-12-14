import arcpy
import arcpy.mapping
arcpy.env.workspace = r'C:\Python27\GE\Input'
arcpy.env.overwriteOutput=True
shape_data=arcpy.ListFeatureClasses()
print (shape_data)
point_data=r'C:\Python27\GE\Input\buildings.shp' #point_data=shape_data[4]

buffer_distance="100 Meters"
road=r'C:\Python27\GE\Input\roads.shp'
buffer_output=r'C:\Python27\GE\Output\Point_Buffer\point_out.shp'

arcpy.Buffer_analysis(point_data,buffer_output,buffer_distance,dissolve_option='All')

pdf_output=r'C:\Python27\GE\Buffer output map\point_buffer_map.pdf'

mxd_path=r"C:\Python27\GE\Map\Blank_map.mxd"

intersect_out=r'C:\Python27\GE\Output\Intersection\Intersect_out.shp'

arcpy.Intersect_analysis([buffer_output,road],intersect_out)
print("Intersection analysis Successful")

mxd= arcpy.mapping.MapDocument(mxd_path)

df=arcpy.mapping.ListDataFrames(mxd)[0]
buffer_layer=arcpy.mapping.Layer(buffer_output)
intersection_layer=arcpy.mapping.Layer(intersect_out)


arcpy.mapping.AddLayer(df,buffer_layer,add_position="TOP")
arcpy.mapping.AddLayer(df,intersection_layer,add_position='TOP')
arcpy.mapping.ExportToPDF(mxd,pdf_output)