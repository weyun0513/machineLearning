# machineLearning

package deepLearning;
import net.sf.javaml.core.Dataset;import net.sf.javaml.classification.Classifier;
import net.sf.javaml.core.Dataset;
import net.sf.javaml.core.Instance;
import net.sf.javaml.tools.data.FileHandler;
import java.io.File;import java.util.ArrayList;import java.util.List;
import libsvm.LibSVM;
public class SvmStockProj { public static void main(String[] args) throws Exception {

        /* Load a data set */      List<Integer> com5=new ArrayList<Integer>();  List<Integer> com10=new ArrayList<Integer>();  List<Double> priceTodayArr=new ArrayList<Double>();  List<Double> priceBArr=new ArrayList<Double>();  List<Double> everyReturnArr=new ArrayList<Double>();  double closePrice,closePriceNext,returnRate;  File file = new File(System.getProperty("user.dir")+"/bin/resources/stock.data");        Dataset data = FileHandler.loadDataset(file, 7, ",");        int count=1500;        Instance inst1=null;        for(int i=0;i<count;i++){         System.out.println(i+"*************** ");         inst1=data.get(i);         closePrice=inst1.value(1);//         System.out.println("closePrice"+closePrice);         for(int j=i+1;j<count;j++){         priceTodayArr.add(closePrice);         closePriceNext=data.get(j).value(1);         returnRate=(closePriceNext-closePrice)/closePrice;         System.out.println(j+": "+closePriceNext+"-"+closePrice+"="+returnRate);         if(returnRate<-0.05){          com5.add(j);         }                  if(returnRate>=0.1){          com10.add(j);         }         if(com10.get(0)<com5.get(0)){          inst1.setClassValue(1);         }else  if(com10.size()>0&&com5.size()==0){          inst1.setClassValue(1);         }else{          inst1.setClassValue(0);         }        }                 }        /*
         * Contruct a LibSVM classifier with default settings.
         */        FileHandler.exportDataset( data,new File("c:/data2"), false) ;        Classifier svm = new LibSVM();
        svm.buildClassifier(data);

        /*
         * Load a data set, this can be a different one, but we will use the
         * same one.
         */
        Dataset dataForClassification = FileHandler.loadDataset(file,8, ",");
        /* Counters for correct and wrong predictions. */
        int correct = 0, wrong = 0,i=0;
        /* Classify all instances and check with the correct class values */        Instance inst=null;//        for (Instance inst : dataForClassification) {//         i++;        for(int testi=1500;i<1700;i++){         inst=dataForClassification.get(testi);            Object predictedClassValue = svm.classify(inst);
            Object realClassValue = inst.classValue();
            if (predictedClassValue.equals(realClassValue)){                correct++;
            }else             System.out.println("i: "+i+"=>"+(String)predictedClassValue);
                wrong++;
        }
        System.out.println("Correct predictions  " + correct);
        System.out.println("Wrong predictions " + wrong);

    }
}
