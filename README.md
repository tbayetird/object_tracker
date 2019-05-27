# Traqueur d'objet inspiré du blog pyimagesearch.com

## Utilisation :
Ce traqueur prend en entrée une liste de rectangles à update ((xmin,ymin,xmax,ymax)) sur une image.
Il faut mettre à jour les nouvelles détections d'une image à une autre, donc réentrer une liste de détections et lancer la mise à jour des objets trackés.


exemple :


    ot = ObjectTracker()
    for i in range(len(y_pred_decoded)):
        img=orig_images[i]
        rects=[]
        for box in y_pred_decoded[i]:
            # Transform the predicted bounding boxes for the 300x300 image to the original image dimensions.
            xmin = int(box[2] * orig_images[i].shape[1] / img_width)
            ymin = int(box[3] * orig_images[i].shape[0] / img_height)
            xmax = int(box[4] * orig_images[i].shape[1] / img_width)
            ymax = int(box[5] * orig_images[i].shape[0] / img_height)
            color = colors[int(box[0])]
            label = '{}: {:.2f}'.format(classes[int(box[0])], box[1])
            cv2.putText(img,label,(xmin,ymin),cv2.FONT_HERSHEY_SIMPLEX,0.5,color,2)
            cv2.rectangle(img,(xmin,ymin),(xmax,ymax),color,2)
            rects.append((xmin,ymin,xmax,ymax))

        if (tracking):
            objects= ot.update(rects)
            numObj=0
            for (objectID,object) in objects.items():
                numObj+=1
                text = "ID {}".format(objectID)
                (xmin,ymin,xmax,ymax)=object.getRect()
                centroid = object.getCentroid()
                predictedCentroid = object.getPredictedCentroid()
                cv2.putText(img,text,(xmax-30,ymin),
                cv2.FONT_HERSHEY_SIMPLEX,0.5,(40,40,255),2)
                cv2.circle(img,(centroid[0],centroid[1]),4,(0,255,0),-1)
                cv2.circle(img,(predictedCentroid[0],predictedCentroid[1]),4,(0,0,255),-1)
                cv2.line(img,(centroid[0],centroid[1]),
                        (predictedCentroid[0],predictedCentroid[1]),(0,0,255))

        out.write(img)
